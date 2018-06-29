---
layout: post
title: CentOS 7 세팅기
date: 2015-11-07 11:14:54
categories: [서버, 리눅스, CentOS]
tags: [서버, 리눅스, CentOS]
comments: true
---
### 나무 위키 CentOS 소개
* <https://namu.wiki/w/%EC%84%BC%ED%8A%B8OS>

저는 Ubuntu LTS 버전으로 서비스 하려 했으나... 퍼블리셔 및 주변의 권유로 CentOS로 세팅해서, 서비스 하기로 했습니다.

CentOS에 대한 핵심 정리를 인용합니다.

RHEL의 소스를 기반으로 만들어지며 철저하게 최신 버전의 RHEL을 미러링하는데 중점을 둔다. 단, 상표권은 회사가 가져가는 GPL의 특성상 레드햇의 트레이트 마크와 로고를 그대로 쓸 경우 상표권 분쟁이 있을 수 있기 때문에 레드햇이 소유하고 있는 레드햇 트레이드마크와 로고는 제거, 그리고 그 자리에 센트OS 고유의 로고를 대신 넣으면 완성. 이 때문에 버전도 RHEL과 똑같이 나간다. 덤으로 센트OS에서 말하는 "북미 엔터프라이즈 소프트웨어 벤더"은 레드햇을 지칭한다.

docker를 이용할 수도 있었지만, CentOS에 대한 이해도를 높이는 차원에서 직접 세팅해보았습니다.
그 과정에서의 기록들을 간단하게 공유해봅니다.

### CentOS7
* Setup
    * <https://www.linux.co.kr/home2/board/subbs/board.php?bo_table=lecture&wr_id=1823>
* Usage
    * [위치 확인] (http://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%AA%85%EB%A0%B9%EC%96%B4_%EC%9C%84%EC%B9%98_%ED%99%95%EC%9D%B8)
    * 폴더 내 용량 확인
        * <code>du -hs *</code>
    * 디스크 용량 확인
        * <code>df -h</code>
* SSH
    * ssh login error
        * <http://ask.xmodulo.com/sshd-error-could-not-load-host-key.html>
* Network
    * cent os 7 방화벽 [http://www.conory.com/note_linux/42477]
        * <code>firewall-cmd --permanent --zone=public --add-port=포트번호/tcp</code>
        * <code>firewall-cmd --reload</code>
    * [포트 확인](http://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%A1%9C%EC%BB%AC%EC%84%9C%EB%B2%84_%EC%97%B4%EB%A6%B0_%ED%8F%AC%ED%8A%B8_%ED%99%95%EC%9D%B8)
    *  SSH,TELNET, FTP 설치 및 운용
        * <http://webdevnovice.tistory.com/5>
* vnc
    * <http://lahuman.jabsiri.co.kr/92>
* Rails
    * rbenv 설치
        * <https://rorlab.gitbooks.io/railsguidebook/content/contents/rbenv.html>
    * rails 설치
        * <https://www.vultr.com/docs/install-ruby-on-rails-with-rbenv-on-centos-7>
    * 버전 강제 [현상은 주로 gem 오류]
        * <code>rbenv install 2.1.5</code>
        * <code>rbenv local 2.1.5</code>
        * <code>rbenv rehash</code>
    * execJS runtime error
        * CentOS node.js install
            * <http://seraphmate.tistory.com/100>
    * pg gem 설치 오류
        * ​<http://​stackoverflow.com/questions/13702417/postgresql-gem-pg-was-unable-to-install>
    * rails 설치시 nokogiri 오류
        * <http://www.nokogiri.org/tutorials/installing_nokogiri.html>
    * rails 4.2.4 이후 외부 접속 안되는 오류
        * 기본 bind가 localhost(=127.0.0.1)
            * 0.0.0.0으로 외부 접근 가능하게 bind 해야 함.
                * <http://serverfault.com/questions/625841/cant-access-ports-assigned-to-rails-4-2-but-4-04-works-fine>
    * Base64 load error
        * /usr/local/share/gems/gems/activesupport-4.2.4/lib/active_support/dependencies.rb:274:in `require': No such file to load -- Base64 (LoadError)
            * Base64 -> base64. on linux error. [대소문자 구분]
    * bundler/setup load error
        * <http://stackoverflow.com/questions/19061774/cannot-load-such-file-bundler-setup-loaderror>
        * <http://stackoverflow.com/questions/25437817/dont-run-bundler-as-root-what-is-the-exact-difference-made-by-using-root>
* Redis
    * 설치
        * <http://lovedev.tistory.com/?page=3>
    * Replication
        * 개념
            * <http://crystalcube.co.kr/176>
        * 설치
            * <http://develop.sunshiny.co.kr/1005>
* Git
    * 설치
        * ​<http://artwook.tistory.com/261>
* 작업 취소 명령
    * <http://ecogeo.tistory.com/276>
* resolving conflrict
    * ​<http://githowto.com/resolving_conflicts>
    * <http://stackoverflow.com/questions/1146973/how-do-i-revert-all-local-changes-in-git-managed-project-to-previous-state>
* svn
    * 설치
        * <​http://zetawiki.com/wiki/SVN_%EC%84%9C%EB%B2%84_%EC%84%A4%EC%A0%95>
    * 시작/중지
        * <http://zetawiki.com/wiki/Svnserve_%EC%8B%9C%EC%9E%91/%EC%A4%91%EC%A7%80>
* Nginx
    * 설치
        * <https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7>
    * thin 연동
        * <http://10apps.tistory.com/142>
        * <https://github.com/mconf/mconf-web/wiki/Nginx-and-Thin>
        * <http://blog.mohitkanwal.com/blog/2013/04/10/deploying-rails-on-nginx-and-thin/>
    * 포트나 루트 폴더 변경
        * <http://stackoverflow.com/questions/10674867/nginx-default-public-www-location>
    * 디렉토리 리스트화
        * <http://khmirage.tistory.com/416>
    * 403 forbidden error
        * <​https://www.digitalocean.com/community/questions/nginx-403-forbidden--2>
* PostgreSQL
    * 설치
        * <​https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-ruby-on-rails-application-on-centos-7>
    * auth failed error
        * <​http://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge>
        * <http://stackoverflow.com/questions/9987171/rails-3-2-fatal-peer-authentication-failed-for-user-pgerror>
    * 외부 접속 허용하기
        * <http://m.blog.naver.com/ships95/220237438650>

* Jenkins
    * 설치
        * ​<http://lifeandshell.com/installing-jenkins-on-centos-7/>
    * sudo 권한 부여
        * <http://stackoverflow.com/questions/29926773/run-shell-command-in-jenkins-as-root-user>
    * 보안 설정 초기화
        * <http://it.i88.ca/2013/04/reset-password-of-jenkins.html>
* Cron [스케쥴러]
    * 사용법
        * <http://fruitdev.tistory.com/10>
* FTP
    * 설치
        * <http://bj001kim.blogspot.kr/2015/05/centos-7-ftp.html>
    * 설정 파일 설명
        * <http://2factor.tistory.com/96>
    * 500 OOPS: vsftpd: refusing to run with writable root inside chroot()
        * <http://webdir.tistory.com/198>
    * 상위 폴더로 이동 제한
        * <http://egloos.zum.cㅇom/parkkun/v/3074093>