---
layout: post
title: CentOS 7 Rails 세팅
date: 2016-02-08 11:14:54
categories: [Ruby, Rails]
tags: [Ruby, Rails, CentOS]
comments: true
---
### rbenv 설치 [ruby, rails]
* 설치
    * sudo yum update
    * sudo yum install git
    * sudo yum groupinstall -y 'development tools'
    * sudo yum install -y gcc-c++ glibc-headers openssl-devel readline libyaml-devel readline-devel zlib zlib-devel  sqlite-devel
    * sudo yum install -y glibc-devel libffi-devel
    * git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
    * git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
* 환경 변수 설정
    >export PATH="$HOME/.rbenv/bin:$PATH"  
    >eval "$(rbenv init -)"

### ruby 설치
* rbenv install 2.3.0
* rbenv global 2.3.0
* rbenv rehash
* ruby -v

### rails 설치
* gem 설치
    * gem install rails
* postgresql
    * sudo yum install postgresql-server postgresql-contrib postgresql-devel
    * sudo postgresql-setup initdb
    * nano /var/lib/pgsql/data/pg_hba.conf
* 외부 접속 허용하기 [pg_hba.conf]
    > host    all     all     0.0.0.0/0       password
    >#rails와의 연결을 위해 md5로 암호화 설정
    >local   all             all                                     md5  
    >host    all             all             127.0.0.1/32            md5
* 외부 접속 허용하기 2 [postgresql.conf]
    * nano /var/lib/pgsql/data/postgresql.conf
        >#listen_address = 'localhost' 에서 listen_address = '*'로 수정. # 주석 풀기
* 계정 생성
    * postgres로 로그인
        * sudo su - postgres 
    * Create a PostgreSQL superuser user with this command (substitute the highlighted word with your own username):
        * createuser -s pguser
    * To set a password for the database user, enter the PostgreSQL console with this command: 
        * psql
    * The PostgreSQL console is indicated by the postgres=# prompt. At the PostgreSQL prompt, enter this command to set the password for the database user that you created:
        * \password pguser
    * Enter your desired password at the prompt, and confirm it. Now you may exit the PostgreSQL console by entering this command: 
        * \q 
    * Now that your PostgreSQL user is set up, switch back to your normal user:
        * exit
* 방화벽 개방
    * firewall-cmd --permanent --add-port=5432/tcp
    * firewall-cmd --reload
* redis
    * download & make
        * cd /usr/local/src 
        * wget http://redis.googlecode.com/files/redis-2.4.17.tar.gz
        * tar xvfz redis-2.4.17.tar.gz
        * cd redis-2.4.17
        * make -j4 && make install  -j4
    * install
        * cd utils
        * ./install_server.sh
    * test
        * redis-cli
            * ping
            * PONG [PONG이 안오면 실패]

### 머신 초기 세팅 [디스크 할당 및 폴더 설정]
* 디스크 목록 보기
    * fdisk -l
* 디스크 할당
    * fdisk /dev/[디스크명]
        * 이후 옵션은 <http://yonoo88.tistory.com/651> 참조
* 디스크 초기화 [ext4로 초기화] 아래 디스크3은 nextmv에서 제공해준 디스크가 500gb씩 분리되어있는 3개의 디스크만 사용할 것 이므로.
    * mkfs.ext4 /dev/xvdg1
    * mkfs.ext4 /dev/xvde1
    * mkfs.ext4 /dev/xvdc1
* ext 폴더 생성
    * cd /usr/local
        * mkdir ext1
        * mkdir ext2
        * mkdir ext3     
* 디스크 마운트
    * mount -t ext4 /dev/xvdg1 /usr/local/ext1
    * mount -t ext4 /dev/xvde1 /usr/local/ext2
    * mount -t ext4 /dev/xvdc1 /usr/local/ext3
* db 데이터 복사
    * cp -r /var/lib/pgsql/data /usr/local/ext2
* 폴더 권한 설정
    * chmod -R 700 /usr/local/ext2
    * chown -R postgres:postgres /usr/local/ext2
* PGDATA 경로 수정. 
    * nano /usr/lib/systemd/system/postgresql.service
    * /usr/local/ext2/data
    * systemctl daemon-reload
    * service postgresql restart
    * 해당 작업 이후에는 설정 파일 경로가  /usr/local/ext2/data 로 바뀌는 점에 유의하자.
* node.js 설치
    * 필요한 패키지 설치
        * yum install gcc gcc-c++
        * yum install openssl-devel
        * yum install make
        * node.js wget
        * cd /usr/src
        * wget http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
    * 압축 해제
        * tar zxvf node-v0.10.22.tar.gz
        * cd node-v0.10.22
    * 설치
        * ./configure
        * make && make install
* 저장소 받아오기
    * cd /usr/share
    * svn checkout svn://[저장소경로] [받아올이름]
* nginx with thin
    * thin 설정 파일 생성. [서버 10개]
        * thin config -C /usr/share/thin -c /usr/share/web_server --servers 10 -e production 
    * 로그 경로 수정.
        * nano /usr/share/thin
        * log: "/usr/local/ext1/log/thin.log" 로 수정.
* nginx 설치
    * sudo yum install epel-release
    * sudo yum install nginx
    * <https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7>
* nginx 설정 추가
* ​주의 : /usr/share/web_server/log 폴더 생성 되어 있어야 nginx service 가동 가능.
    >cd /etc/nginx  
    >mkdir sites-enabled  
    >nano sites-enabled/thin  
    >upstream thin {  
    >   server 127.0.0.1:3000;  
    >   server 127.0.0.1:3001;  
    >   server 127.0.0.1:3002;  
    >   server 127.0.0.1:3003;  
    >   server 127.0.0.1:3004;  
    >   server 127.0.0.1:3005;  
    >   server 127.0.0.1:3006;  
    >   server 127.0.0.1:3007;  
    >   server 127.0.0.1:3008;  
    >   server 127.0.0.1:3009;  
    >}  
    >  
    >server {  
    >   listen 포트;  
    >   server_name 서버이름;  
    >  
    >   access_log /usr/local/ext1/web_server/log/access.log;  
    >   error_log /usr/local/ext1/web_server/log/error.log;  
    >  
    >   root   /usr/share/web_server/public/;  
    >   index  index.html;  
    >  
    >   location / {  
    >       proxy_set_header  X-Real-IP  $remote_addr;  
    >       proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;  
    >       proxy_set_header Host $http_host;  
    >       proxy_redirect off;  
    >  
    >       if (-f $request_filename/index.html) {  
    >           rewrite (.*) $1/index.html break;  
    >       }  
    >  
    >       if (-f $request_filename.html) {  
    >           rewrite (.*) $1.html break;  
    >       }  
    >  
    >       if (!-f $request_filename) {  
    >           proxy_pass http://thin;  
    >           break;  
    >       }  
    >   }  
    >}  
    >nano /etc/nginx/nginx.conf  
    >#맨 끝 } 안에 추가  
    >include sites-enabled/*;
* 서버 가동
    * thin -C /usr/share/thin start
* nginx 재 가동
    * service nginx restart