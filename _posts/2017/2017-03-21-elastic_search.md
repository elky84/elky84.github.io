---
layout: post
title: ElasticSearch
date: 2017-03-21 11:14:54
categories: 엘라스틱서치 ElasticSearch
Tags: 엘라스틱서치 ElasticSearch
comments: true
---
# ElasticSearch

## 목적
* 검색을 위해 색인을 만들고, 색인을 바탕으로 빠른 검색을 위한 어플리케이션.
    * 엘라스틱서치를 색인 기능이 추가된 NoSQL DBMS라고 생각하면 이해하기 쉬울 수도 있다.
* 장점
    * 분산 시스템
        * 엘라스틱 서치는 여러 개의 노드로 구성되는 분산 시스템
        * 노드는 데이터를 색인하고 검색을 수행하는 단위 프로세스
        * 기존 노드에 새 노드를 실행하여 연결하는 것만으로 확장 가능
        * 데이터는 각 노드에 분산 저장
        * 복사본을 유지하여 각종 충돌로부터 노드 데이터 보호
        * DISCOVERY를 내장하여 별도의 분산 시스템 관리자 불필요
    * 높은 가용성 (HIGH AVAILABILITY)
        * 엘라스틱 서치는 하나 이상의 노드로 구성
        * 각 노드는 1개 이상의 데이터 원본과 복사본을 서로 다른 위치에 나누어 저장
        * 노드가 종료되거나 실행에 실패할 경우 다른 노드로 데이터 이동 항상 일정한 데이터 복사본의 개수를 유지하여 높은 가용성과 안정성 보장
    * 멀티 테넌시 (MULTY TENANCY)
        * 데이터는 여러 개로 분리된 인덱스들에 그룹으로 저장 (인덱스 = RDBMS의 데이터베이스에 대응)
        * 서로 다른 인덱스의 데이터를 하나의 질의로 검색하여 하나의 출력으로 도출 가능
* 기본 개념과 용어
    * 인덱스/타입/문서(index/type/document)
        * 엘라스틱서치의 데이터 계층이다. MySQL로 치면 database/table/row에 대응하는 개념이다. REST API 에서 문서를 표현할 때는 /news/article/10000 과 같이 표현한다. 이 때 news가 인덱스, article이 타입, 10000이 문서다.
    * 필드(field)
        * 엘라스틱서치 문서는 JSON이다. JSON의 각 프로퍼티를 엘라스틱서치에서 필드라고 부른다. MySQL로 치면 column에 대응하는 개념이다.
    * 매핑(mapping)
        * 인덱스/타입/문서의 규칙을 정의한 것이다. 사용자가 마음대로 정의할 수 있다. MySQL로 치면 스키마다. NoSQL에도 스키마가 있냐고? 물론 있다.
    * 색인(index)
        * 엘라스틱서치가 문서를 검색할 수 있도록 색인 데이터를 만들어두는 과정을 말한다. 데이터 계층의 index와 동일한 단어인데 문맥에 따라 다르게 쓰이므로 주의
    * 색인(index)
        * 위의 index의 명사형. 명사형으로 쓰일 때는 색인 작업을 거쳐 만들어진 색인 데이터를 가리킨다. 엘라스틱서치가 다루는 실제 저장된 데이터라고 할 수 있다. index 한 단어가 3가지 의미로 쓰이는 셈이다. 한국어 교재에서는 문맥에 따라 인덱스(데이터 계층)와 색인(색인 과정, 색인 결과)이라는 두 개의 단어로 구분하고 있다.
    * 클러스터/노드(cluster/node)
        * 여러 대의 서버를 묶어서 구동하기 위해 사용되는 개념이다. 각 서버가 노드, 서버의 묶음이 클러스터.
    * 샤드/복사본(shard/replica)
        * 엘라스틱서치는 색인 데이터를 하나의 물리적 데이터 공간에만 저장하는 게 아니라, 여러 개의 저장공간에 나누거나 복사할 수 있다. RAID0, RAID1과 비슷한 개념이다. shard는 성능향상을 위해 데이터를 여러 물리적 공간에 나눠 저장하는 것이고, replica는 한 노드가 실패했을 때도 검색서비스 제공이 가능하도록 데이터를 여러 물리적 공간(그리고 노드)에 복제해 두는 것이다.
    * QueryDSL
        * JSON으로 표현되는 엘라스틱서치의 검색 문법이다.
        
### 소개
* <https://www.slideshare.net/seunghyuneom/elastic-search-52724188>

### Plugin

* 추천
    * <http://maong.tistory.com/194>
* 은전한닢
* <http://eunjeon.blogspot.kr/>
* <https://bitbucket.org/eunjeon/seunjeon/raw/master/elasticsearch/>
    * 최신 버전이 5.1.1
        * <https://www.elastic.co/downloads/past-releases/elasticsearch-5-1-1>
    * 5.2에 설치
        * <http://www.kwangsiklee.com/ko/2017/02/%EC%97%98%EB%9D%BC%EC%8A%A4%ED%8B%B1%EC%84%9C%EC%B9%98-5-2-0%EC%97%90-%EC%9D%80%EC%A0%84%ED%95%9C%EB%8B%A2-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/>

### LogStash
* JDBC
    * <http://geniedev.tistory.com/6>
        * 문서 참고시 주의할점은, 해당 문서와 다르게 인덱스 생성할 때 POST말고 PUT 써야 한다.
            * 버전이 올라가면서 변경된 걸로 추정
* Apache + Redis
    * <http://jinhokwon.tistory.com/134>
