---
layout: post
title: 게임 서버에서의 DB 사용 전략 - 02 NoSQL
date: 2022-08-21 00:00:00
categories: [서버]
tags: [게임 서버, 데이터베이스, NoSQL]
comments: true
---

## 개요

RDB는 데이터 저장소를 선택할 때, 가장 기본이 되는 선택지이다.

한국에서 가장 자주 선택되는 웹 서버 프레임 워크인 Spring만 봐도 과거에는 JDBC나 MyBatis, 지금은 JPA가 필수 기술에 포함되는 것만 봐도 알 수 있을 것이다.

---

## 살펴보기

그렇다면 NoSQL의 입지는 어떨까?
요즘은 정말 많은 채용 공고에서 MongoDB를 볼 수 있는데, 이는 한국에서도 NoSQL은 일반적인 선택지로 떠올랐음을 증명한다고 볼 수 있을 것이다.

그렇다면 MongoDB를 RDB 사용하듯 사용할 수 있을까? 모든 면에서 대체가 가능할 까?

몇가지 측면에서 살펴보자.

### 트랜잭션

기본적인 접근이 스키마리스이며, 트랜잭션이 있긴 하고, MongoDB의 경우 Multi Document Transactions이 도입된 지 몇년 됐지만, RDB에 익숙한 우리가 기대하는 방식으로 적극 사용해선 안된다. 성능 저하도 큰편이며, 이를 기반으로 스키마 설계를 하지 말라고 권고 되고 있다.

즉 또한 컬렉션 전체나, 분할 컬렉션에서는 지원되지 않으며, 제한 시간은 5초이므로 RDBMS와 동일하게 연상하고 사용하는 일은 지양 해야 할 것이다.

<aside markdown="1">
💡 [Transactions — MongoDB Manual](https://www.mongodb.com/docs/manual/core/transactions/)

**IMPORTANT!**

In most cases, multi-document transaction incurs a greater performance cost over single document writes, and the availability of multi-document transactions should not be a replacement for effective schema design. For many scenarios, the denormalized data model (embedded documents and arrays) <https://docs.mongodb.com/manual/core/data-model-design/#std-label-data-modeling-embedding>


will continue to be optimal for your data and use cases. That is, for many scenarios, modeling your data appropriately will minimize the need for multi-document transactions.

For additional transactions usage considerations (such as runtime limit and oplog size limit), see also


[Production Considerations](https://docs.mongodb.com/manual/core/transactions-production-consideration/)


**중요!**


대부분의 경우 다중 문서 트랜잭션은 단일 문서 쓰기보다 더 큰 성능 비용을 초래하며 다중 문서 트랜잭션의 가용성이 효과적인 스키마 설계를 대체해서는 안 됩니다. 


많은 시나리오에서 [비정규화된 데이터 모델(임베디드 문서 및 배열)](https://docs.mongodb.com/manual/core/data-model-design/#std-label-data-modeling-embedding) 은 데이터 및 사용 사례에 계속 최적입니다. 


즉, 많은 시나리오에서 데이터를 적절하게 모델링하면 다중 문서 트랜잭션의 필요성을 최소화할 수 있습니다.


추가 트랜잭션 사용 고려 사항(예: 런타임 제한 및 oplog 크기 제한)은 프로덕션 [고려 사항을](https://docs.mongodb.com/manual/core/transactions-production-consideration/) [참조 하십시오](https://docs.mongodb.com/manual/core/transactions-production-consideration/)

</aside>

<aside markdown="1">
💡 [Azure Cosmos DB API for MongoDB에서 다중 문서 트랜잭션 사용 - Microsoft Docs](https://docs.microsoft.com/ko-kr/azure/cosmos-db/mongodb/use-multi-document-transactions)

**요구 사항**

다중 문서 트랜잭션은 API 버전 4.0의 분할되지 않은 컬렉션 내에서 지원됩니다. 다중 문서 트랜잭션은 컬렉션 전체 또는 4.0의 분할된 컬렉션에서 지원되지 않습니다. 트랜잭션의 제한 시간은 5초로 고정되어 있습니다.


유선 프로토콜 버전 4.0 이상을 지원하는 모든 드라이버는 Azure Cosmos DB API for MongoDB 다중 문서 트랜잭션을 지원합니다.

</aside>

### 성능 지향형 데이터 베이스

그렇다면 NoSQL의 강점은 무엇일까? 바로 **속도=성능**이다.

게임 서버에서도 RDB는 성능 이슈를 야기한 사례가 수도 없이 많다.

그래서 memory mapped file이나, 실질적으로 join이나 stored procedure등의 DB기능을 사용하지 않은 프로젝트도 많았다. 

또한 redis같은 범용 cache 는 물론이며, 자체 cache 서버를 구현해가며 RDB로 가는 직접적인 트래픽을 줄이는 데에 치중해왔던 시기도 있다.

성능을 위해 많은 것을 희생할 바에는 NoSQL이 적절한 대안이 될 수 있다.

RDB를 사용하며 DB를 통한 무결성에 의존하는 상황이 아니라면, NoSQL이 성능과 기능에 대한 밸런스를 충분히 훌륭하게 잡아 줄 수 있다.

NoSQL을 쓰기 위해선, 데이터의 무결성에 대한 검사와 활용을 로직 레이어에서 일부 커부해주어야 한다.

Relational (관계) 형 디비가 아닌 만큼 관계에 덜 치중하고, 일관성 (Consistency)에 대한 집착을 버린다면 큰 성능적 이득과, 고 가용성을 얻을 수 있을 것이라는 주장이고 이 주장이 설득력을 얻어, 현재 꽤나 많은 서비스들이 Hybrid로 구성되고 있다.

이는 다수의 채용 공고에서도 알 수 있는데, 현재 기준 MongoDB는 MySQL처럼 대 다수 회사에서 적극 활용 중이며, 이에 대한 경험이나 이해도를 우대(또는 기본 요건으로 요구)하는 것을 채용 공고를 통해 알 수 있다.

이렇듯 일반화 된 기술이 될 수 있었던 것은, 트랜잭션이 과도한 연산량과 대기 시간을 야기하는 기술이기 때문이다.

결국 대용량 서비스를 지향할 수록 RDB만으로 지탱 하기 어렵다는 얘기라는 거다.

---

### 적절한 사용처

그래서 NoSQL을 쓰면 어떻게 생각하고 어떻게 사용해야 할 까?

내가 쿼리를 수행하고 있는 중에도 연관 데이터가 쌓일 수 있음을 이해해야 한다. 데이터를 꺼낼 때 연관 관계를 위한 값을 조건으로 걸거나, 가져온 데이터의 짝이 맞는지 체크하거나, 시간 등을 조건으로 걸어 정합성이 맞는 데이터만 가져와야 한다.

애초에 NoSQL을 쓴 다는 것은 성능을 위함이므로, 비동기 드라이버를 이용하게 되는데, 비동기로 오는 응답을 위에 언급한 대로 조합해서 내려주거나, frontend가 여러 자원에 대한 응답 결과를 모아서 처리하는 방식으로 구성되어야 한다.

물론 사용하는 NoSQL이 join을 지원한다면 이를 이용해도 된다.

---

### 집계 연산

NoSQL 모두의 특성은 아니지만, 집계 (aggregation) 연산시에도 큰 장점이 있다.

즉, 통계 데이터를 뽑아 내는 데에 큰 강점이 있다. null 데이터가 존재할 때의 비효율도 RDB에 비해 압도적으로 낮다.

[Mongodb 장단점, 활용시 고민할 사항들 - 엘키의 주절 주절 (elky84.github.io)](https://elky84.github.io/2018/09/26/mongodb/)

[Mongodb 서버 구축 및 아키텍쳐 - 엘키의 주절 주절 (elky84.github.io)](https://elky84.github.io/2018/09/26/mongodb_architecture/)

[혁신적인 게임 개발을 위한 데이터베이스 선택 (MongoDB) (sarc.io)](https://sarc.io/index.php/nosql/2255-2022-01-16-23-44-40)

---

### 여전한 인덱스의 중요성

잊지 말아야 할 점은 스키마리스이지, 스키마가 여전히 중요하고 인덱스의 중요성은 RDB 못지 않다는 점이다.

쉽게 적응하기 위해선 RDB의 테이블과 같은 개념으로 접근하되, null 컬럼이 유연하다는 접근 정도로 시작하면 좋다.

그리고 검색되는 조건에 따라 인덱스를 맞게 생성해야 한다. 복합 조건 쿼리를 이용한다면, 복합 인덱스를 만들어 주어야 하는 것이다. 자세한 정리는 다음 글을 읽으면 좋다.

[MongoDB index 개념과 indexing 전략 :: 정찡이 (tistory.com)](https://ryu-e.tistory.com/1)

---

## 마치며

지금까지 읽었다면 바로 적용할 사용처는 매우 손쉽고, 기존에 RDB에 의존했던 데이터는 아직도 조금 저항감이 있을 것이다.

고민을 덜 하고 적용할 사용처인 로그, 히스토리, 랭킹 등과 같은 데이터에 적용해보자. 

NoSQL은 이미 성숙한 기술이고, 우리가 생각하는 것 이상의 많은 곳에서, 많은 트래픽을 소화하고 있다.

아직 안쓰고 있다면, NoSQL을, (상대적으로) 학습 코스트 낮게 도입하고 싶다면, MongoDB를 도입하는 걸 추천한다.