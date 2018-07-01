---
layout: post
title: Mongodb 팁
date: 2018-06-27 00:00:00
categories: [Mongodb, NoSQL, 빅데이터]
tags: [Mongodb, NoSQL, 빅데이터]
comments: true
---

mongodb를 로그 디비로 운용하며 생긴 이슈 및 쿼리 튜닝에 대한 내용을 간략하게 정리해보았습니다.
운용을 검토중이시거나 운용 중이신 분들, NoSQL로 선정 고려중이신 분께 도움이 되면 좋겠네요.

* 인덱스 작성시 쿼리와 1:1 대응 시켜라.
    * 적합 인덱스를 못찾았을 경우 rejectPlan을 타게 되는데 이 때, 매우 비효율적인 연산이 일어날 확률이 컸다.
* 쿼리에 index가 의도대로 동작하지 않으면 hint를 줘라.
    * 그럼에도 느리다면 대게는 hint를 잘못 준 것.
* group 연산시 인덱스에 없는 값을 필요로 할 경우, 해당 값에 대해 찾느라 디스크까지 내려갔다 오는 비용과 인덱스 못찾는 비용으로 소요 시간이 급격히 커질 수 있음에 유의하라.
    * 즉 group 연산 내에 들어가는 컬럼이 모두 index에 존재해야 성능상 이득이 크다.
        * group, match, project등의 연산도 모두 index에 존재해야 한다는 점에 유의하라.
* 메모리를 확보하라. 인덱스가 메모리 영역에 올라와서 연산하기 때문에, VM이던 리얼 머신이건 램이 중요하다.
* explain을 통해서 인덱스를 어떻게 타는지 확인하라.

* 참고 자료
    * 쿼리 튜닝
	    * [MongoDB 쿼리 튜닝](https://blog.naver.com/PostView.nhn?blogId=suresofttech&logNo=221096609752&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)
    * 성능 개선 팁
	    * [MongoDB 성능 개선 및 팁](https://webisfree.com/2018-03-12/mongodb-%EC%84%B1%EB%8A%A5-%EA%B0%9C%EC%84%A0-%EB%B0%8F-%ED%8C%81)
	    * [월간 10억 PV를 지지하기 위한 MongoDB Tip](https://jacking75.github.io/choiheungbae/%EB%AC%B8%EC%84%9C/%EC%9B%94%EA%B0%84%2010%EC%96%B5%20PV%EB%A5%BC%20%EC%A7%80%EC%A7%80%ED%95%98%EA%B8%B0%20%EC%9C%84%ED%95%9C%20MongoDB%20Tip.pdf)
