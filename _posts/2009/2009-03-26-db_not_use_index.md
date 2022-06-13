---
layout: post
title: 인덱스가 있지만 인덱스를 안 타는 경우
date: 2009-03-26 11:14:54
categories: [데이터베이스]
tags: [RDB, Database, 데이터베이스, MS-SQL, SQL Server]
comments: true
---

1. 인덱스  컬럼의 변형
    select * from table  where LOWER(name)  ='word';
    select * from table  where idx - 1 = 5;
    이 처럼 인덱스에 변형을 가하게 되면, DBMS가 인덱스를 이용하지 않는다.
2. NOT 또는 IN 연산자 사용
    NOT일 경우 무조건 인덱스를 안타는 것이 아니다.
    NOT일 경우에도 인덱스를 타긴 타지만, 일반적으로, NOT에 사용된 값이 아닌 데이터의 비율이 높은 경우가 많기 때문에 인덱스를 타지 않는 경우가 많다.
    마찬가지로 IN일 경우에도, IN에 포함된 데이터들의 비율이 매우 높다면 FULL SCAN을 하는 것이 낫다고 DBMS가 판단하면 인덱스를 타지 않는다.
3. 와일드 카드 LIKE문장에서 범위를 전체를 지정시
    select * from table  where name like '%word'; 
    문자열로 이루어진 값을 인덱스로 잡았을 때, %가 앞쪽에 사용되면 정렬 순서를 사용할 수 없으므로 테이블 FULL SCAN이 이루어진다.

    select * from table  where name like 'word%'; 
    당연한 얘기지만 쿼리가 이런 경우 인덱스를 탄다. 문자열 정렬 순서를 그대로 이용할 수 있기 때문이다.
4. 복합 컬럼 index에서 조건이 잘못되여 index 가 적용 되지 못하는경우
    select * from table where name = 'word' or idx = 5
    name과 idx가 둘다 인덱스가 걸려있는 경우라해도, DBMS가 최적의 OR 조건을 뽑기 힘들어, FULL SCAN 하는 경우가 많다.
5. Optimizer 의 선택
    select * from table  where name ='word' and  id ='elky'; 
    인덱스가 name 과   id로 2개가 있을 경우 id나 name 인덱스 중 하나가 선택될수도 있고, 둘다 선택 될 수도있다.
    어떤 방식으로 선택하는냐가 속도에 중요할수도있다. 즉 실행 계획을 추적해서 원하는 결과가 나오도록 관리가 필요하다