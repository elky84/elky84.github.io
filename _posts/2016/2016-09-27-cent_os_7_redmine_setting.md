---
layout: post
title: CentOS 7 Redmine 세팅
date: 2016-09-27 11:14:54
categories: [Ruby, Rails, Redmine, CentOS]
tags: [Ruby, Rails, Redmine, CentOS]
comments: true
---
### CentOS7 Redmine 설치
* 방화벽 개방
    ~~~ bash
    firewall-cmd --permanent --zone=public --add-port=3000/tcp
    firewall-cmd --reload​
    ~~~
* GCC 설치
    ~~~ bash
    yum -y install gcc cpp gcc-c++ compat-gcc-34 gcc-gfortran flex​
    ~~~
* Ruby와 Passenger 빌드에 필요한 헤더파일
    ~~~ bash
    yum install openssl-devel readline-devel zlib-devel curl-devel libyaml-devel​
    ~~~
* Mysql과 헤더파일
    ~~~ bash
    yum install mysql-server mysql-devel
    ~~~
* Apache과 헤더파일
    ~~~ bash
    yum install httpd httpd-devel
    ~~~
* ImageMagick과 헤더파일
    ~~~ bash
    yum install ImageMagick ImageMagick-devel
    ~~~
* Ruby설치
    * 소스다운로드
        * <http://www.ruby-lang.org/ko/downloads/>
        * <http://cache.ruby-lang.org/pub/ruby/2.3/>
    * 설치
        ~~~ bash
        cd /usr/local
        wget http://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.0.tar.gz
        tar zxvf ruby-2.3.0.tar.gz
        cd ruby-2.3.0
        ./configure --disable-install-doc
        make
        make install
        make clean
        ~~~
* Bundler 설치
    ~~~ bash
    gem install bundler --no-rdoc --no-ri
    ~~~
* redmine 설치 3.16 -- 안정화버젼이라서 선택함
    ~~~ bash
    cd /usr/local
    wget http://www.redmine.org/releases/redmine-3.1.6.tar.gz
    tar zxvf redmine-3.1.6.tar.gz
    mv redmine-3.1.6 /usr/local/redmine
    ~~~
* 설정 파일
    ~~~ bash
    cd /usr/local/redmine/config
    cp database.yml.example database.yml
    vi database.yml
    ~~~
    ~~~ yaml
    production:  
        adapter: mysql  
        database: redmine  
        host: localhost  
        username: redmine  
        password: redmine  
        encoding: utf8 
    ~~~
* Gem Package 설치
    ~~~ bash
    bundle install --without development test
    ~~~
* 테이블 생성 및 초기 데이터 입력
    ~~~ bash
    cd /usr/local/redmine
    rake generate_secret_token
    RAILS_ENV=production rake db:migrate
    RAILS_ENV=production rake redmine:load_default_data
    #한국어 ko 입력
    ~~~
* 구동
    ~~~ bash
    bundle exec rails server webrick -e production -d -b 0.0.0.0
    # 데몬으로 실행 여기까지 끝
    ~~~
* 중지
    ~~~ bash
    kill -INT $(cat tmp/pids/server.pid)
    ~~~
* FAQ 
    * 설치 중에 bundle install 시 에러 내용을 자세히 보면 마지막 문구에 뭐를 설치하라고 gem 뭐뭐뭐 이렇게 명령어 나오는데 복사해서 그걸 그대로 입력해서 설치 후 bundle install 다시 실행해야 한다. 
    * bundle install은 /usr/local/redmine 에서 해야 한다
    * gem install 안될시에 gem query --remote -n 이름-d -a 이 명령어로 버젼 높은걸로 찾아서 인스톨
* redmine 테마
    * /usr/local/redmine/public/theme 이쪽폴더에 넣고 재시작
 * redmine 플러그인
    * /usr/local/redmine/plugins 폴더에 넣고 /usr/local/redmine 에서 하단을 실행하여 플러그인을 추가해야한다.
        ~~~ bash
        bundle install --without development test
        rake redmine:plugins:migrate RAILS_ENV=production
        ~~~
* redmine 구글이용하여 메일보내기
    ~~~ bash
    cd /usr/local/redmine/config
    ~~~

    ~~~ bash
    # configuration.yml.example 을 configuration.yml로 만든후
    nano configuration.yml
    ~~~
    ~~~ yaml 
    production:
       email_delivery:  
       delivery_method: :smtp  
       smtp_settings:  
       enable_starttls_auto: true  
       address: "smtp.gmail.com"  
       port: 587  
       domain: "smtp.gmail.com"
       authentication: :plain  
       user_name: "계정명@gmail.com"  
       password: "비밀번호"
    ~~~
    * 이렇게 해도 메일이 안 보내진다면 원인은 구글 계정에 대한 보안 설정 오류 일 가능성이 높음.
        * <https://www.google.com/settings/security/lesssecureapps>
        * 보안 수준 낮은거 설정하고 2차 인증 설정하지 안 해야지 메일 보내집니다
* Redmine 관리
    * <http://www.whatwant.com/367>
