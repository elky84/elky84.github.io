---
layout: post
title: PostgresSQL 요약 정리
date: 2016-02-08 11:14:54
categories: [RDB]
tags: [PostgresSQL, RDB, Database, 데이터베이스]
comments: true
---
### 한눈에 살펴보는 PostgreSQL
* <http://d2.naver.com/helloworld/227936>

### 장점
* <http://adbanced.tistory.com/24>
* <http://devx.tistory.com/34>

### pgadmin [management tool]
* <http://www.pgadmin.org/download/>

### Function
* 사용자 정의 함수
    * <http://blog.daum.net/aswip/8429507>

### Syntax
* 기본 문법
    * <http://database.sarang.net/database/postgres/tutorial/lecture/book1.htm>
* select insert into
    * <http://www.chesnok.com/daily/2013/11/19/everyday-postgres-insert-with-select/comment-page-1/>
* select into
    * <http://acpi.tistory.com/119>
* 생성/ 삭제
    * <http://articles.slicehost.com/2009/5/7/postgresql-creating-and-dropping-databases>

### 프로파일링
* <http://daewonyoon.tistory.com/162>

### 쿼리 측정 및 통계
* DB 쿼리 통계
    * <http://postgresql.kr/docs/9.3/monitoring-stats.html>
* 테이블 통계 정보 보기
    * select * from pg_stat_all_tables
* 실행중인 쿼리 확인 및 취소
    * <http://blog.gaerae.com/2015/09/postgresql-pg-stat-activity.html>
* 미사용 인덱스 조회
    * <http://blog.gaerae.com/2015/09/postgresql-index.html>
        >SELECT schemaname,  
        >relname, indexrelname,  
        >pg_size_pretty(pg_relation_size(indexrelid::regclass)) AS index_size,  
        >idx_scan,  
        >idx_tup_read,  
        >idx_tup_fetch  
        >FROM pg_stat_user_indexes  
        >ORDER BY idx_scan ASC;
* 백업 복구
* pg_dump 참고
    * <http://www.thegeekstuff.com/2009/01/how-to-backup-and-restore-postgres-database-using-pg_dump-and-psql/>
    * <http://blueamor.tistory.com/227>
    * <http://www.postgresql.org/docs/8.0/static/backup.html>
    * <http://ktdsoss.tistory.com/251>
* 외부 접속
    * 인증 권한 - vi /var/lib/pgsql/data/pg_hba.conf
        >local   all              all                                              trust  
        >host   all              all              0.0.0.0/0               trust  
        >listen 주소 수정 - vi /var/lib/pgsql/data/postgresql.conf  
        >listen_addresses = '*'  
        >port   5432  
        >max_connections = 100  
        >위 두 정보를 수정한 뒤, 반드시 postgresql 서비스를 재시작 해주어야 한다.  
* 참고
    * <https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-ruby-on-rails-application-on-centos-7>
    * <http://acet.pe.kr/246>
