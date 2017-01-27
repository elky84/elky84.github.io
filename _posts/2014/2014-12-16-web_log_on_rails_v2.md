---
layout: post
title: 웹 로그 서버 구축기 with rails V2
date: 2014-12-16 11:14:54
categories: Ruby Rails
comments: true
---

# 개발 목표
자동으로 파일로 남겨져 있는 로그를 분석해, 시스템에 영향을 주는 작업을 알려주기 위해 개발 됐다.
파일로 남겨져 있는 log를 db으로 밀어 넣는 작업은 log_shipper가 담당한다.
db에 있는 데이터를 조건에 맞게 검색해서 보여주는 역할을 하는 web_server를 만들고자 의도했다.
* 실제 쿼리 작성 비용과 web_server의 API 개발 비용이 크게 차이 나지 않음을 느낄 수 있다.
서비스 중에도 볼 수 있도록, REST API 서버로서의 기능도 수행한다.
* 관련된 기능은 개발용으로 put method를 만들어둔 것을 이용하면 된다.

 
# 최종 목표
통지 기능
서비스에 문제가 있었는지를, 유저 보고가 오지 않더라도 알 수 있게끔 한다.
base line 설정 기능.
major patch시나, 무언가 의심될 때는 익일 보고 모드가 아닌 observer mode로 동작 시키자.
  
# 동작 방식
각 서버는 파일로 로그를 떨군다.
local에 남겨진 로그를 log shipper가 db로 운송한다.
* option에 따라 web으로 직접 전송하기도 한다. (observer mode)
원하는 목표를 산출하기 위한 몇 가지 기능을 Ruby on rails를 통해 이용한다.
 
# 구현 issue
db 병목에 대한 대처. (기본적으로 rdb에 대한 접근이 실시간으로 이루어져, 이에 대한 비용 문제가 있다.)
* partioning 할까?
* raw data를 직접 쌓는 지금 방식에서 개선이 필요하다.
* raw data를 합산해서 http call을 하고, 그 데이터를 바로 쌓지 않고 find and update 하는 구조로 가야 할 듯 싶다.
mvc를 1:1 대응으로만 구현하면 성능의 문제에 휘말림.
* db cahching을 model을 통해 구현해야 함.
text file을 line별 parsing을 하고 split 하려던 중, 오류 발생.
* ruby invalid byte sequence in utf 8
* ruby도 encoding 문제에서 자유롭지 못함.
* 사실 이건 모든 프로그래밍 언어의 문제...정확히는 윈도우의 문자열 코드 페이지 처리에서 생기는 문제.
* 해당 문자열에 force_encoding("iso-8859-1").encoding("UTF-8")을 하니 처리 되기 시작.
* 헌데, 읽혀진 문자열에 유효하지 않은 공백이 포함됨.
text 파일이 ascii, utf-8은 정상적으로 읽힘. 헌데, unicode option의 text file만 안 읽힘.
* 각종 encoding option으로 해결 안됨.
* line 별 encoding 을 시도.
* 첫 번째 라인만 encoding 됨. 두 번째 라인부터 꼬임.
* line별 encoding을 확인해보았더니, 두 번째 라인부터는 CP949 (ascii code page 949)로 인식되는 것을 확인.
* 파일 전체를 열고 버퍼 전체를 encoding 해보았더니 정상적으로 읽힘.
* 해결!
 
# production 이슈
mysql2의 사용법을 따라 해도 gem install부터 안됨.
첫 번째 문제는 x64용 mysqlconnector를 이용한 것이 원인.
x86으로 바꾸고 시도. Gem install에는 성공함.
 
Install은 됐으나 정상 동작하지 않음.
구글링 해보니 루비 설치폴더에 libmysql.dll을 수동으로 넣어줘야 된다고 함.
해당 파일을 C:\mysql-connector-c-6.1.5-win32\lib 에서 해당 파일을 C:\RailsInstaller\Ruby2.0.0\bin 로 옮겨주니,
 
gem mysql2가 수동 설치 되기 시작.

    gem install mysql2 --platform=ruby -- '--with-mysql-dir="C:\mysql-connector-c-6.1.5-win32"'
 
gemfile에 gem 'activerecord-mysql2-adapter' 를 포함하고 시도.

    rake db:migrate

에서 문제
 
gemfile에 gem 'mysql2' 를 포함하고 시도하니 해결
 
# 현재 미구현 사항
통지 기능
* ruby script와 연동해 메일 발송 기능을 지원?
* ror엔 trigger 기능이 없음. 정해진 시간에 http call을 해줄 애는 누구로..?
log_shipper를 call 할 시점과 주체는?
로그 남기는 것을 float이나 double같은 실수 형으로 남기도록 수정. (count말고 수행 시간 같은 것)
테이블 인덱스 설정
* IP, Message를 pair key로 잡으면 될 듯?

해결 목록
partioning table 처리 (DB 병목 대비)
* partioning 처리 자체를 update or insert로 처리 했으므로 DB 병목의 가능성은 줄였고, 대신 데이터 수신 시 비용이 증가.
Log_shipping 병렬화
* 1차 thread 사용
* 2차 thread pool 사용 (concurrent-ruby gem의 기능 사용)