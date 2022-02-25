---
layout: post
title: 예외처리 01 - 결과 코드 주고 받기
date: 2022-01-27 00:00:00
categories: [Exception, Exception Process, Result Code]
tags: [Exception, Exception Process, Result Code]
comments: true
---

예외 처리. 우선 순위가 떨어지기 쉬운 주제다.

내가 주니어 시기를 게임 개발에서만 보내왔고, 또 속도 전이 중시되던 2000년대 중반에 주니어 경험을 했다보니 (슬프게도 지금도 아니라고 말하긴 어렵지만), 우선 개발 속도를 끌어 올리고 시간이 남으면 예외 처리를 하거나, QA나, 플레이 테스트 중에 과도하게 불편할 경우에만 한정적으로 예외처리를 하게 됐다.

그리고 패킷 크기를 줄이는 데에 집착적인 시기도 겪다보니 공용 헤더에 결과 코드를 넣고 싶어하지 않은 경우도 많았다. 

그리고 시기적으로도 예외 처리의 중요성을 덜 중요하게 여겨졌던 것도 사실이다. (나 역시 그랬으니)

그렇다보니, 프로토콜 공용 헤더에서 실패 이유를 전달하게끔 구현되어있지 않았다.

이렇게 특정 응답에서만 한정적으로 구현할 경우 문제가, 기존 응답에서 실패 응답을 추가 할 경우 프로토콜 확장이 반드시 필요해진다.

또한 글로벌한 실패 코드 핸들링을 하기 어렵다. 

---

내가 여러 서비스를 개발/운용해본 결과 모든 응답에 (예외 발생시도 커버해주는 Global Exception Handler 등록을 통해서) 오류 원인을 전달하는 것이 좋다.

보통 게임에서는 프로토콜 코드를 어떤 방식으로던 share하게 되고, protobuf나 flatbuffers 등에서 모두 protocol definition 파일을 통한 enum 공유를 지원한다.

웹에서 Frontend와 Backend간의 통신에서 결과 코드를 쉐어하기 위해서는 Backend 정의 데이터를 정적 파일로 전달 받던, 결과 코드가 무엇 무엇이 있는지에 대한 API를 호출해 가져와서 사용하는 방법 등을 통해 어떠한 실패 종류가 있는지 응답 받고, 그 상황을 어떻게 핸들링해야 하는가를 구현해야 한다.

결과 **코드를 포함한 응답 예시**

~~~json
{
  "data": {
    "region": "KR",
    "summonerId": "GsAtyvFdEdR3zyKLqHA--UztFnq6m6ZXz03L-tGNuJnJZw",
    "accountId": "VfbTYUdqT2IfWTrSoB84d0GwyUOJftRVZDrVQNc7K-E",
    "name": "Elky",
    "nameLower": "elky",
    "puuid": "lwSflLnWfYTfp1uMmDBByjr0azx98eXEEi9RF8EWnUUtsaW8V2pI56FoyBP6-g78M8uQ7ZGVnMDfKg",
    "level": 148,
    "tracking": true,
    "trackingGameId": null,
    "id": "6145a4cec04e6604b03d8a2d",
    "created": "2021-09-18T17:35:25.813+09:00",
    "updated": "2022-01-23T20:37:03.697+09:00"
  },
  "leagueEntries": [
    {
      "inactive": false,
      "freshBlood": false,
      "veteran": false,
      "hotStreak": true,
      "losses": 6,
      "wins": 20,
      "miniSeries": null,
      "leaguePoints": 1,
      "tier": "GOLD",
      "queueType": "RANKED_SOLO_5x5",
      "summonerName": "Elky",
      "summonerId": "GsAtyvFdEdR3zyKLqHA--UztFnq6m6ZXz03L-tGNuJnJZw",
      "leagueId": "abad2fac-ae52-4e2b-bf7d-e5b59d6f4b7d",
      "rank": "IV",
      "id": "5f701e40bf1c44e2ea41e08d",
      "created": "2021-09-18T17:35:22.863+09:00",
	    "updated": "2022-01-23T20:36:03.697+09:00"
    },
    {
      "inactive": false,
      "freshBlood": true,
      "veteran": false,
      "hotStreak": true,
      "losses": 33,
      "wins": 29,
      "miniSeries": null,
      "leaguePoints": 85,
      "tier": "SILVER",
      "queueType": "RANKED_FLEX_SR",
      "summonerName": "Elky",
      "summonerId": "GsAtyvFdEdR3zyKLqHA--UztFnq6m6ZXz03L-tGNuJnJZw",
      "leagueId": "4899ab5c-2766-49b1-b0d9-a19c47f877dc",
      "rank": "IV",
      "id": "5f701e40bf1c44e2ea41e08e",
      "created": "2021-09-18T17:35:22.891+09:00",
	    "updated": "2022-01-23T20:36:03.697+09:00"
    }
	],
  "resultCode": {
    "description": "성공",
    "id": 1
  }
}
~~~

개발 (DEBUG 모드, TEST 환경) **공용 헤더 부분**

~~~json
{
  "resultCode": {
    "description": "성공",
    "id": 1
  }
}
~~~

**프로덕션 공용 헤더 부분**

~~~json
{
  "resultCode": {
    "id": 1
  }
}
~~~

위와 같은 규격이 결과 코드다. 개발 환경에서의 응답과, 실제 서비스 상황에서의 응답은 조금 다르게 구성되어있는데, 이 과정에서 데이터가 좀 더 많게 구성되어있다. 송수신 데이터 양을 줄이기 위해서 다음과 같이 구성하는 것도 가능할 것이다.

**좀 더** 간소화된 프로덕션 **공용 헤더 부분**

~~~json
{
  "resultCode": 1
}
~~~

---

결과 코드에 따른 동작에 대해서 고민해보자. 모든 (성공이 아닌) 예외 상황에 대해서, 팝업만 띄우면 된다면 너무나 쉬울 것이다.

하지만 종종 원인이나 상황에 따라서 해야 될 동작이 달라진다.

로그인(=sign in)만 봤을 때도, 결과 코드에 따라서 다른 동작을 해주어야 한다. 성공 코드는 기존 페이지로 이동, 실패 코드 다수는 팝업 메시지, 내부 오류 또는 특정 실패 코드의 경우 점검 페이지로 보내야 될 것이다.

상품 구매도 마찬가지로 결제 수단 오류인지, 결제 취소인지, 제품 품절인지 (결제 처리 중에 재고 소진) 등에 따라서 다른 처리가 필요하다.

결과 코드에 맞춰 동작을 정의하면, 예외 처리하는 과정에서 드는 고민 중 하나인 예외 처리 때문에 코드가 복잡해지는 상황도 줄일 수 있게 된다.

결과 코드에 맞게 실행되는 Reactor 패턴을 이용하면 코드상 결합도를 줄일 수 있는 장점도 얻어갈 수 있다.

요청이 뭐였냐에 따라서 결과 코드별 다른 동작을 해주어야 하는 상황이 된다면 어느정도는 복잡도가 올라가긴하는데, 이는 리액터 패턴의 단점이라기보다는 요구 사항 자체가 가진 복잡도라고 봐도 무방하다. 

---

결과 코드를 통한 명시적 응답이 가진 단점은 거의 없다. 응답 데이터 크기가 커진다 정도? 응답 크기도 성공시 데이터를 비워두는 식의 규칙을 가져간다면 대 부분 상황에서의 응답 데이터 크기는 동일해진다.

또한 텍스트 응답 대신, 약속된 결과 코드(정수형 데이터)로 응답한다면 이 또한 크기를 줄일 수 있다.

결과적으로 모든 응답이 같은 규격의 결과 코드 규칙을 가지고, 이에 대한 글로벌적 처리를 할 수 있다는 것은 장점이 훨씬 많기 때문에 어떤식으로든 주고 받아야, 각 파트의 관점이 아닌 하나의 제품으로서의 완성도를 높일 수 있다.

또한 문제 해결 주체나,디버깅 시간을 크게 감소 시켜주는 원동력이 되기도 하고 말이다.

아직 응답 코드에 결과 코드나 결과 내용에 대한 응답이 없다면 추가해보는 것을 추천한다.