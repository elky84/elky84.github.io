---
layout: post
title: MSA 그리고 API Gateway
date: 2018-09-21 00:00:00
categories: [MSA, API Gateawy]
tags: [MSA, API Gateawy]
comments: true
---

내가 웹 서버 개발에 어렵지 않게 적응 할 수 있었던 데에는 웹과 게임 서버간에 의외로 비슷한 요소가 많았던 것이 크게 작용했다.

클라이언트와 서버의 연동이 어떻게 이루어지는가, 사용자에게 개방되는 요소가 무엇 인가, 이벤트를 어떻게 주고 받을 것인가 등등 사실상 이질감이 느껴지는 요소가 아주 많지는 않았다.

또 한가지 비슷한 요소가 있었는데, 백엔드 서비스간의 연동이다. 그 연동 관계를 **서비스 아키텍쳐**라고 부른다.

네트워크 모델과 유사한 면이 있는데, 이는 피어와 피어간의 통신 구조에 따라서 관리 이슈, 통신 코스트, 예외 상황, 장애 대응 등의 다양한 면에서 비슷한 접근이 가능하다.

이런 과정에서, [**MSA(Micro Service Architecture)**](https://zetawiki.com/wiki/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4)가 대두되었는데, [**SOA (Service Oriented Architecture)**](https://zetawiki.com/wiki/%EC%84%9C%EB%B9%84%EC%8A%A4_%EC%A7%80%ED%96%A5_%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98_SOA)의 발전형이라고 봐도 무방하다.

기본적으로 [**일체형 아키텍쳐(Monolithic Architecture)**](https://zetawiki.com/wiki/%EB%AA%A8%EB%86%80%EB%A6%AC%EC%8B%9D_%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98)와 상반되는 개념으로써, 기능을 한 수레에 담지 않는 모델의 장점을 기반으로 설득력을 얻어가는 구조다.

---

**일체형 아키텍쳐**의 장점은 구현이 쉽다. 모든 데이터를 내가 보유하고 있으므로, 애초에 모든 일을 할 수 있다.

반면 성능 이슈 발생시 해결이 어렵다. 어떠한 기능으로 인한 성능 문제가 발생했는지 측정하기가 상대적으로 어려울 뿐더러, 특정 기능에 한정해 성능 개선이 필요할 때 유연하게 대비하기 어렵게 구현되기 쉽다. 또한 배포와 변경, 기동 시간에 단점이 있기 때문에 확장이 상대적으로 어려운 편에 속한다. 

뿐만 아니라 로직의 결합도가 생기기 쉽고, 이는 스팟 포인트 증가, 블러킹 포인트 증가로 인한 성능 저하로 이루어지기 쉽고, 자연스레 버그 발생 확률이 증가하는 단점이 있다.

![일체형 아키텍쳐와 MSA 비교](/img/2018/decentralised-data.png)

[출처: martinfowler.com](http://martinfowler.com/articles/microservices.html)


이를 개선하기 위해 **MSA**는 서비스 단위로 서버를 구성하고, 수평 확장에 유연하므로 서비스 단위별로 스케일 인/아웃을 유연하게 구성하며, 결합도가 낮으므로 버그 발생 확률이 줄어들며, 테스트 단위도 작아질 수 있다.

성능 측정, 개선도 기능단위로 이루어지므로 명확하며, 필요한 만큼만 성능 확장을 할 수 있고 아키텍쳐 구축 과정에서 이뤄진 동적 스케일링 대비로 인한 안정적 서비스는 덤이다.

다만 여러 개 서비스의 데이터를 조합해서 사용해야 될 때의 문제, endpoint 관리의 문제, 적정한 서비스 단위를 규정하는 문제, 인증/인가 로직을 서비스마다 구현해야 되는 문제 등이 존재한다.

이를 해결하기 위해서 **API Gateway**를 사용하게 되는데, endpoint 관리 문제, 진입점 일치로 인한 인증/인가 처리의 유연함, 통계/로그 기록이 편해지는 장점, 과도한 트래픽 방지를 위한 Ratelimit 처리 등으로 인한 쪼개진 각 서비스의 이슈를 해결 하는 데에 집중한다.

![API Gateway](/img/2018/api_gateway.png)

단점으론 대다수의 서비스가 API Gateway를 통해 통신하게 되므로, API Gateway 자체에 장애가 발생하면 서비스 전체에 지장을 준다. 그래서 이를 대비하기 위한 여러가지 대안이 마련되고 있고, 그 중 최근 화두가 되고 있고, 가장 유명한 것은 Circuit Breaker 패턴을 기반으로 구현된 Netflix Hystrix라고 할 수 있다.

---

**Netflix**의 **Hystrix**에 대해서 간단히 설명하고 넘어가자면, API Gateway의 구현 자체는 HTTP Request 응답을 대행하는 Proxy 역할을 담당하게 되므로 Request Proxy처리하는 동안 Request 요청을 잡고 있어야 한다. 

대신해서 호출한 대행 요청이 오래 걸리게 된다면, 요청에 대한 처리속도가 저하되게 된다. 이렇게 밀리는 현상이 지속 되다 보면 전체적으로 응답 시간 저하가 이뤄지고 서비스에 지장을 주는 문제가 발생한다.

그래서 Timeout을 작게 잡아서, 특정 서버의 처리 시간 지연이 다른 요청에 영향을 주지 않게 처리해야한다.

이를 위해서 Timeout이 일정 수준 이상 발생한 서버는 요청 대상에서 격리 시킨다거나, Timeout 자체를 작게 설정해야 해서 영향을 덜 받게 만든다거나 하는 작업을 통해 API Gateway 자체의 Throughput 저하를 막아야 한다.

이 역할을 Hystrix가 담당해준다고 생각하면 된다. API Gateway에는 치명적인 단점인 장애의 전파지점이 SPOF (=Single Point Of Failure)가 되는 문제가 있으나, 이를 극복하기 위한 Netflix의 보완책이라고 볼 수 있다.

---

이와 별개로, API Gateway를 운용해보니 여러가지 생각이 들었다. 별개의 글에서 좀 더 자세히 설명하게 되겠지만, API Gateway에서 중요한 부분은 정확한 계측을 통해 안정적인 환경을 각 서비스에 전달하는 역할이다. 

이를 위해서는 Public API Gateway는 사용자 요청을 수행하고 (Frontend 단에서 호출되게 처리. 인증/인가를 담당), Private API Gateway (내부 서버간 통신에서만 사용. ACL등의 최소화한 보안 처리. 통계의 분리)를 구성하는 것도 하나의 방법이라는 생각이 든다.

---

### API Gateway가 해주면 좋은 역할
- 인증
- 인가 
- Ratelimit (사용자별, 서비스간 별개로 관리)
- Endpoint 관리 (Routing, LoadBalancer, Dynamic Endpoint Management)
- API 통계
- API 로깅
- API Request, Response 형식 검사 및 일관성 유지

이외에도 공통적으로 처리되어야 할 작업들을 API Gateway가 처리해주면 좋다.

다른 글에서 좀 더 자세히 다루겠지만, Netflix API Gateway 4종 세트 (Zuul, Ribbon, Eureka, Hystrix)는 API Gateway 모델의 장점 극대화, 단점 최소화를 이뤄냈다는 점에서 어떻게 구성했는지 배워볼 필요가 있다.

### Netflix API Gateway의 핵심 기능
- Zuul : Router and Filter
- Ribbon : Load Balancer
- Eureka : Registry Service
- Hystrix : Latency and fault tolerance

![Netflix Zuul](/img/2018/zuul_02.png)

이 모듈들은 특별한 편집 없이 Annotation과 설정 파일만으로도 구축할 수 있다는 점이 더 큰 장점이다.
위에 언급한 대다수의 역할은 Zuul을 연동하고 동작 시킬 API Gateway 서버의 Filter에서 처리할 수 있다.

![Zuul Filters](/img/2018/zuul_01.png)

---

MSA에서 자주 겪는 문제는 모든 서비스에 같은 기능이 필요할 때의 중복 문제가 크게 발생한다. 이에 대한 버전별 서비스 이슈도 있고, endpoint 변경에 대해  유연한 대처도 어렵다.

이를 해결하기 위한 대안이 바로 API Gateway였고, 다양한 시도와 경험을 바탕으로 장단점이 밝혀졌다. 그리고, 그 것들을 극복하기 위한 대안들이 마련되며 MSA의 마스터 피스가 되었고, 현재로썬 Netflix Zuul이 안정적인 MSA 서비스 모델의 이상향으로 여겨지고 있다.

특히 Netflix의 어마어마한 사용자를 MSA로써 소화하는 모델에서 이용된 핵심 컴포넌트이기에, 그 노하우를 배우고 개선할 만 하다고 여겨진다.

모르긴 몰라도 많은 회사들이 내부적으로 자체적인 API Gateway를 개발하고, 운용할 것이다. 실제로 언어별로 다양하게 API Gateway 모듈이나 서비스가 존재하는 걸 보면, MSA에서 가장 중요한 역할은 API Gateway를 잘 구성하고 운용하는 데에 있다고 보는 듯 하다.

SOA에서 이어진 MSA의 확산, API Gateway 등장, API Gateway의 단점 개선으로 계속 진화하고 있는 MSA가 앞으로 어떻게 진화할지 궁금해진다.