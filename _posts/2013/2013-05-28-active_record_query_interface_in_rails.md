---
layout: post
title: Active Record Query Interface
date: 2013-05-28 11:14:54
categories: Ruby Rails
tags: Ruby Rails
comments: true
---

[액티브 레코드 쿼리 인터페이스](http://rubykr.github.io/rails_guides/active_record_querying.html)

튜토리얼만 잘 읽고 가이드만 잘 따라가도 평타를 칠 수 있는 언어! 루비....인데, 액티브 쿼리 인터페이스 읽다말고 find_by_sql을 발견 한 후, find_by_sql 위주로 작업을 했더니 몇가지 문제가 있었습니다.

현재의 세팅환경은, 다음과 같습니다.

develop : sqlite
production : mysql

이렇게 쓰다보니, 특정 dbms 종속형 쿼리를 작성 했을시에 특정환경에선 동작하지 않는 기능을 만들어버리는 것이었죠.

ROR의 액티브 쿼리 인터페이스란걸 알고보니 어지간한건 직접 쿼리 안짜고 가능하더군요!!

### 아래는 ROR에서 지원하는 메소드 종류입니다.
* where
* select
* group
* order
* limit
* offset
* joins
* includes
* lock
* readonly
* from
* having

뭐 여타 DBMS에서도 흔히 볼 수 있는 구문들이므로...자세한 설명은 패스하겠습니다.

만약 find_by_sql을 많이 쓰고 계신다면, 가급적 액티브 쿼리 인터페이스를 쓰시는 것이 여러모로 장점이 많지 않을까 싶네요.