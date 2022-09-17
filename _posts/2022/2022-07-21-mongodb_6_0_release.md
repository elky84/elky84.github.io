---
layout: post
title: MongoDB 6.0 릴리즈와 7가지 핵심 변경 사항
date: 2022-07-21 00:00:00
categories: [NoSQL]
tags: [MongoDB]
comments: true
---

## 7가지 MongoDB 6.0 업그레이드 이유
1. **Even more support for working with time series data**
    - 시계열 데이터 작업에 대한 더 많은 지원
2. **A better way to build event-driven architectures**
    - 이벤트 기반 아키텍처를 구축하는 더 좋은 방법
3. **Deeper insights from enriched queries**
    - 풍부한 쿼리에서 더 깊은 인사이트
4. **More operators, less work**
    - 더 많은 운영자, 적은 작업
5. **More resilient operations**
    - 더 탄력적인 작업
6. **Additional data security and operational efficiency**
    - 추가 데이터 보안 및 운영 효율성
7.  **A smoother search experience and seamless data sync**
    - 더 매끄러운 검색 경험 및 끊김 없는 자료 동기화

### 버전별 핵심 차이 [위키 참고](https://en.wikipedia.org/wiki/MongoDB)

|Version|Release Data|Feature notes|
|---|---|---|
|v1.0|August 2009| |
|v1.2|December 2009|more indexes per collection, faster index creation map/reduce, stored JavaScript functions, configurable fsync time, several small features and fixes|
|v1.6|August 2010|production-ready sharding, replica sets, support for IPv6|
|v2.4|March 2013|enhanced geospatial support, switch to V8 JavaScript engine, security enhancements, text search (beta), hashed index|
|v2.6|April 8, 2014|aggregation enhancements, text-search integration, query-engine improvements, new write-operation protocol, security enhancements|
|v3.0|March 3, 2015|WiredTiger storage engine support, pluggable storage engine API, SCRAM-SHA-1 authentication, improved explain functionality, MongoDB Ops Manager|
|v3.2|December 8, 2015|WiredTiger storage engine by default replication election enhancements, config servers as replica sets, readConcern, document validations, moved from V8 to SpiderMonkey|
|v3.4|November 29, 2016|linearizable read concerns, views, collation|
|v4.0|June 2018|transactions|
|v5.0|July 13, 2021|future-proofs versioned API, client-side field level encryption, live resharding, time series support|
|v6.0|July 2022| Even more support for working with time series data, A better way to build event-driven architectures, Deeper insights from enriched queries, More operators, less work, More resilient operations, Additional data security and operational efficiency, A smoother search experience and seamless data sync|

## 소감
현재 MongoDB는 NoSQL 중에서 압도적인 포지션을 갖고 있다. 물론 Cassandra (ScyllaDB), Couchbase 등도 장단점이 차이가 뚜렷하지만, 그럼에도 조금 더 RDBMS에서 넘어가기 좋은 수많은 개념적, 실용적 장점을 가지고 있어 많은 유저를 끌어들이고 있다.

범용 DB인 만큼 특수 목적에서는 단점이 몇개 있었으나, 지속적 개선을 하고 있다. 그래서 사실상 다목적 보조 DB 혹은 아키텍트가 일부 받쳐 준다면, RDBMS 없이 MongoDB만으로 서비스를 구축 가능한 수준까지 발전하고 있다.

물론 Trasaction이 2018년 추가 됐지만 그럼에도 우리가 기대하는 Transaction과는 차이 나는 부분이 존재하다보니, RDBMS를 전혀 안 쓰기는 어려울 수 있지만, 그럼에도 NoSQL은 대용량 서비스에서 필수 불가결한 존재가 되어가고 있다.

이 글을 보고 관심이 가신다면, 토이 프로젝트나 서비스를 보조하는 부분에서라도 써보시는 건 어떨까 싶다.

## 공식 문서
* [MongoDB 6.0 Now Available!](https://www.mongodb.com/blog/post/big-reasons-upgrade-mongodb-6-0)
* [7 Big Reasons to Upgrade to MongoDB 6.0](https://www.mongodb.com/blog/post/big-reasons-upgrade-mongodb-6-0)