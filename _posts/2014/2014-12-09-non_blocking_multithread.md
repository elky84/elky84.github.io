---
layout: post
title: non-blocking multithread programming
date: 2014-12-09 11:14:54
categories: [Async, 병렬프로그래밍, 멀티스레드]
tags: [Async, 병렬프로그래밍, 멀티스레드]
comments: true
---

클럭도 물론 중요하지만, 코어가 몇갠지 부터 보는 일이 자연스러워진지도 몇년.

다들 병렬 프로그래밍 잘 하고 계시나요?

서버 프로그래밍을 시작한 2006년부터 지금까지... 멀티 스레드를 다뤄오며, 느낀 것에 대해 이야기해보고자 한다.

# lock
말그대로 잠금. 이 데이터 unlock 될때 까지 쓰지말라는 거다.

- non-blocking
non-blocking이 뭐냐고? 대기하는 상황(blocking) 없이 코드가 수행되는 것을 말한다.

- lock is blocking.
lock이라는 것 자체가 blocking을 위한 녀석이다. 멀티스레드에 적합한 녀석 일리 없다.

- lock-free
container나 데이터에 접근할 때에 lock 객체에 대한 고민없이 사용해도 되는 자료구조나 스레드 모델을 lock-free라고 통칭한다.
좀 더 디테일하게 설명하자면 lock-free container, lock-free design이라 칭해도 되겠다.


# atomic operation
- atomic operation (or atomic instruction)
CAS (http://en.wikipedia.org/wiki/Compare-and-swap) 기반의 명령어 집합을 의미한다.
windows 에선 Interlocked 계열 메소드(http://msdn.microsoft.com/en-us/library/windows/desktop/ms684122(v=vs.85).aspx)를 통해서 지원한다. software적으로 직접 구현도 가능하다. (Interlocked 계열 메소드도 software적 구현)

- limit of atomic operation
atomic operation이 해결해 주는 것도 꽤나 많다. 객체단위 lock보다, CAS는 잠금 시간이 짧다.
다만 그로 인해 성능은 좋아질 지언정 논리상 오류가 없을까? 기본적인 전치검사 후치보장 룰을 깨뜨릴 가능성이 높다.
결국엔 논리적 오류까지 해소하기 위해선 스레드 디자인이 필요하다는 얘기다.

# parelell
- lock and paralell
애초에 lock이란 pararell과 어울리지 않는 단어.
허나 locking을 minimal 하는 목표를 갖고 pararell 하게 구현해보자.

- **reactor, proactor**

* reactor : dispatching based.
* proactor : callback based.

조금 더 자세히 보자면 proactor는 이 작업을 비동기로 수행하고 그 결과를 알려주세요라는 개념이고, reactor는 어떠한 작업이 언제 완료될지 모르니, 작업에 대한 통지가 오길 기다리는 개념이라고 보면 된다.

# thread-design
- entry point
진입점이 명확하면 좋다. 데이터를 발생 시키는 지점부터 고민해야 한다.
내 경험상 수 많은 서버가, entry point의 종류로 network receive, cyclic action (internal looping), system thread (api internal thread) callback, packet process thread 정도가 주를 이룬다.
이 위치를 명확히 파악해두어야, 부하에 대한 tracking이 쉬워진다.

- contact point
각 entry point에서 발생된 event들이 겹치는 지점을 말한다. entry point가 종류별로 여러 스레드로 구성 되어 있을 경우 특히나 contact point가 겹치곤 하는데, 이 위치에 lock을 놓는 전략이 가장 쉽지만, 성능 저하를 일으키는 지점이 되곤한다.

- thread count
스레드 갯수는 경합 확률을 높인다. 하지만, 잘 되어있는 스레드 디자인이 이를 우회할 수 있다.
blocking 되는 상황이 있는 thread와, 할 일이 있다면 쉬지 않고 일하는 non-blocking thread를 파악하고, 그 갯수를 비례해서 조절하는 것이 좋다.

sleep 함수도 마찬가지다. 현재 context를 이관하는 선택은, blocking이 되는 상황이 되고 있다는 것을 전제한다.

- tip
contact point는 필연적으로 lock을 강제당한다.  
contact point를 제거할 수 없다면, contact point를 갈라주거나, locking을 minimize 하는 노력을 기울여야 한다.

- lock minimize
-> contact point가 줄어들면 자연스레 lock을 최소화 해서 사용할 수 있게 된다.
-> contact point를 줄이는 쉬운 팁은 thread마다 다른 instance에 접근하도록 하는 것이다. 
-> 예를 들어 MMORPG에서 지역별로 다른 message worker를 이용하도록 하면, 잠금없이 스레드가 동작할 수 있다.
-> 다른 지역간의 interaction이 일어나는 동작을 개체단위 lock이 아닌 message로 수신해서 처리하는 방식으로 한다면, message queue에 밀어넣는 잠깐의 지연만 걸릴 뿐이다.
-> 조금 더 쉽게 정리하자면, 개체의 소유권을 각 스레드에 두고, 입력과 출력은 message로써 다루면 lock을 줄일 수 있다.


# warning
- event explosion
어떻게 구현하건간에 처리량은 한계가 있다.
이벤트량 자체가 폭발하지 않게끔제어해야 한다.

예를들면 패킷을 수신하는데, 비정상 적인 수의 패킷을 발생 시킨다면, flooding 처리를 해야 한다.
또한 로직에서 아이템 처리를 위해 여러개 티어를 거친다고 생각해보자. 아이템 획득 처리, 아이템 로그, 퀘스트 달성 여부, 이벤트 진행인지 검사 이벤트 등... 모든 컨텐츠의 티어가 잘게 나누어져 있고, 처리 티어들을 한번씩만 거친다고 해도 이벤트량 자체는 많을 수 밖에 없다.

티어를 너무 잘게 쪼갠다면, 데이터를 완성 시키기 위한 과정에서의 이벤트량 자체가 많아지고, 오류 발생의 여지도 늘어난다.


# hot spot
latency는 두 종류가 있다.
1. 절대적인 처리량이 많아서. (event explosion으로 인한 처리량이 많아진 상황도 포함한다)
2. 병목 지점에서 대기가 이루어져, 전체 대기 시간이 길어지는 상황.

이 중 2번의 주 원인은 hot spot에 있다. 
hot spot이 발생했다는 것 자체가, contact point에서의 지연이 발생하거나, contact point에 동시 접근하는 스레드가 많다는 의미다.
다시 한번 강조하자면, thread design이 그래서 중요하다.

# maintain
* profiling
측정하라. entry point부터 processing이 끝난 시점까지 측정하는 것은 너무나도 당연하지만 생각보다 이에 대한 모니터링이 이루어지지 않는 경우도 많다.
이벤트 갯수, 이벤트 처리시간, 오류 발생, 경고 등 지표화 할 수 있는 데이터들을 잘 남겨 두는 것이 중요하다.

프로파일링은 기능의 일부처럼 다뤄져야한다. 

번거롭고, 거추장스러워 문제가 생기고 나서야 확인하는 범행 현장 조사 같은 일이 되어서는 안된다.

* base line
처리 속도와 처리 시간의 목표 지점을 설정하라. 한 패킷이 5ms이하로 동작하리라 목표로 삼는다면, 1스레드당 200개 패킷을 처리할 수 있다는 전제를 세울 수 있다.
처리량을 기준으로, 한 서버당 가용 동접이 설정 가능해진다.

정상, 경고, 임계, 장애등의 수위를 정해놓고 각 수위별로 다른 알람을 받으면 좋다.
또한 runtime에 해당 수치를 낮출 수 있는 대안 (컨텐츠별 block)을 마련해두어 대응할 수 있어야 한다.



스레드 디자인에 대한 내용은 다음 글에서 따로 다뤄보고자 합니다. 꽤나 길어질거 같군요.

제 생각에 대한 이의나 다른 의견, 질문 뭐든지 환영합니다.

이상으로 멀티 스레드 프로그래밍의 퍼포먼스 해결을 위한 팁을 정리해보았습니다.