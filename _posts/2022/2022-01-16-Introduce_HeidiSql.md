---
layout: post
title: HeidiSQL 소개
date: 2022-01-16 00:00:00
categories: [DBMS,RDB,Tool,Recoomend]
tags: [DBMS,RDB,Tool,Recoomend]
comments: true
---

한 어플리케이션의 `DBMS`를 보통은 한개로 통일하는 경우가 많긴하다. 추가적으로 사용하는 경우는 `Metric DB`, `NoSQL`을 섞어쓰는 경우가 많지, `RDB` 계열을 여러개 사용하는 경우는 일반적이지는 않다.

왜냐면 결국엔 튜닝, 최적화, 예외처리 코스트가 크기 때문이다.

또 DB마다 제공되는 툴들이 있다. 그런 툴 들은 해당 DB에 특화된 기능을 지원하고, 기능이 적은만큼 오류도 상대적으로 적은 장점이 있다.

`MySQL`의 `Workbench`, `SQL Server`의 `SQL Server Management Studio`, `PostgreSQL`의 `pgAdmin` 등이 그런 포지션의 툴들이다.

그럼에도 여러 디비를 동시에 사용하는 개발자 군이 존재하는데, 개발 연혁이 길어서 여러 프로젝트를 거치면서 사용한 디비가 다양해 졌다거나, 여러 팀을 지원하는 역할이라거나, 한 프로젝트 내에서 여러 디비를 사용하는 케이스(물론 RDB를 여러 개 쓰기란 흔치 않은 선택이긴 하지만 존재하는걸 본적이 있다)가 그렇다.

이렇듯 다양한 `DBMS`를 접근해야 될 때, 여러개 툴을 설치해서 `DBMS`에 맞게 사용하는 것도 물론 할만한 선택이긴 하지만, 하나의 툴에서 모두 해결해주면 숙련도 측면이나 활용팁에서 장점이 있다.

주요 `RDB` (`MySQL`, `SQL Server`, `PostgreSQL`, `MariaDB`, `SQLite`)를 모두 지원하고, 내장 툴이 지원하지 않는 몇가지 기능을 지원하는 툴인 `HeidiSQL`을 소개하고자 한다.

# [HeidiSQL](https://heidisql.com)

독일 출신 개발자 Ansgar Becker가 개발했다.

![heidisql](/img/2022/heidisql_00.png)

![heidisql](/img/2022/heidisql_01.png)

MAC 유저분들에게는 안타깝게도 Windows만 지원한다.

지원디비 및 지원 방식은 다음과 같다

![heidisql](/img/2022/heidisql_02.png)

## 디비 편의 기능

![heidisql](/img/2022/heidisql_03.png)

### 1. 유지보수

![heidisql](/img/2022/heidisql_04.png)

### 2. 텍스트 찾기

![heidisql](/img/2022/heidisql_05.png)

### 3. 데이터베이스를 SQL로 내보내기

기본 제공 툴에서 지원되는 DB 백업, 복원 기능이 깔끔하게 동작하면 좋겠는데.... `SQL Server`던 `MySQL`이던 백업되는 디비와 복원되는 디비가 다를 경우 복원 불가한 경우가 수두룩하다.

대용량 라이브 디비 마이그레이션이라면 물론 안정성도 지켜주고, 버전 이슈를 해소해주는 전문 마이그레이션 툴을 써야 겠지만, 간단한 알파/베타/라이브 등의 환경에서의 백업/복원을 위해서는 매우 유용한 기능이었다.

![heidisql](/img/2022/heidisql_06.png)

### 4. 벌크 테이블 편집기

![heidisql](/img/2022/heidisql_07.png)

## 기타 편의 기능

### Overview

이외에 **전반적인 기능은 [이 페이지](https://www.heidisql.com/help.php)를 참고**하면 된다.

### 간략한 기타 기능 설명

글로 모든 기능을 표현하긴 쉽지 않으나, 조금 더 설명하자면  조회된 데이터, 전체 테이블 데이터 뷰에서도 조작이 가능하다. 또한 GUI 기반의 필터도 좀 더 편하게 지원한다.

![heidisql](/img/2022/heidisql_08.png)

테이블 생성/편집에 대한 편의 기능을 제공해준다.

![heidisql](/img/2022/heidisql_09.png)

기본 툴을이 보통 제공해주는 쿼리 생성기도 물론 지원한다.

![heidisql](/img/2022/heidisql_10.png)

이외에도 `Create Stored Procedure`, `Create Trigger`,  `Create Schedule Event`, `Import CSV`,  `Import BLOB` 와 같은 기능들도 있다

단점으로는 오픈 소스이다보니, 특정 버전의 호환성 이슈 등으로 크래시도 꽤 있었던 편이고, 기능이 오동작하는 케이스도 종종 있었던 점은 참고 부탁한다.

[Github HeidiSQL](https://github.com/HeidiSQL/HeidiSQL)  해당 Github가 공식이므로, issue 제보를 하면 빠르게 대응해주시기 때문에 큰 불편없이 사용해왔다. 몇번 버그 제보해봤는데, 아주 빠른 대응이 인상적이었다.

만약 본인이 위에 언급한 편의 기능들 중 맘에 드는게 있거나, HeidiSQL이 지원하는 DB중 2개 이상을 사용한다면 (아마도 SQLite는 대부분 테스트 DB로서 사용하거나, 클라이언트 캐시 레이어로 사용하는 일은 많으실 것이다) 고민 해 볼만한 DB 툴이라고 생각한다.