---
layout: post
title: Kubernetis 이야기
date: 2020-01-08 00:00:00
categories: [Docker, Kubernetis, k8s]
tags: [Docker, Kubernetis, k8s]
comments: true
---

쿠버네티스가 이미 너무나 당연한 대세가 된지 꽤 됐으나… 여러가지 속설과 사용해본 분들의 단편적인 감상만으로는 부족했다.

나는 작년이 되서야 제대로 사용해보게 되었는데, 그에 대한 감상과 경험담을 써보도록 하겠다.

직접 사용해본 결과, 확실히 어려운 부분이 존재는 하지만, 대부분의 학습 곡선이 네트워크 구성이고, 나머지 구성요소는 어차피 도커를 쓰던, VM위에 바이너리를 올리던 거쳐야 될 과정들이며, 도커 이미지화에 성공했다면 낮은 학습 곡선이라고 볼 수도 있을 정도의 장점도 많았다.

쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼이다. 쿠버네티스는 선언적 구성과 자동화를 모두 용이하게 해준다. 쿠버네티스는 크고, 빠르게 성장하는 생태계를 가지고 있다. 쿠버네티스 서비스, 기술 지원 및 도구는 어디서나 쉽게 이용할 수 있다.
	
출처: <https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/> 

![배포 환경의 진화](/images/deployment_evolution.png)	

---

**선언적 구성**이라 함은 **YAML과 같은 설정 파일로 설정으로 도커 이미지들을 쉽게 배포할 수 있다는 것**을 말한다.

**자동화**라함은, **적용되어있는 설정을 유지하기 위한 노력을 쿠버네티스가 지원해준다는 것**을 말한다.

당연하지만 YAML 파일 자체는 직접 작성하거나, 작성을 도와주는 도구를 이용해야 한다.

* Argo CD - Declarative GitOps CD for Kubernetes
	* <https://argoproj.github.io/argo-cd/>


레플리카 개수 지정만으로 쉽게 스케일 인-아웃이 가능한데, 이 설정은 당연히 네트웍 트래픽, 도커위에서 운용중인 어플리케이션의 상태, 처리량에 따라 동적으로 결정지어야하며, 서비스 추가를 위해선 수십초 내지는 수백초가 소요될 수 있으므로 임계치를 향해 갈 징조를 파악해서 스케일 아웃 해야하므로, 다양한 모니터링 도구와 연동하는 것이 필요하다.

그 중 하나인 프로메테우스를 비롯한 다양한 도구가 존재하고, 이런 도구를 통한 자동화를 하지 못한다면, 전통적인 배포 환경과 크게 다르지 않게 되므로, 쿠버네티스를 쓰는 이상 꼭 구축해서 오토 스케일 인/아웃 환경을 구성하길 권장한다.

* 프로메테우스
    * <https://github.com/coreos/kube-prometheus>
	* <https://arisu1000.tistory.com/27857>

여기에 더 좋았던 점은 MS의 적극적인 오픈 소스 지원 정책의 일환으로 VS2019 (VS2017에서도 가능했다)에서 도커 지원 메뉴를 클릭하기만해도, 참조 로컬 프로젝트, nuget 패키지등을 끌어와서 빌드해주는 Dockerfile까지 짜잔하고 완성된다는 점이다.

내 경우에는 코드 공유 및 중복 제거를 위해서, 프로젝트를 작게 쪼개서 참조를 관리하고 있으며, 로컬 프로젝트 참조를 해야 하는데, 이를 Dockerfile 작성하는게 꽤나 귀찮고, 실수할 소지가 많다. 심지어 프로젝트 구조가 종종 바뀌기 마련인데, Dockerfile 작성은 꽤나 집중력도 필요하고 테스트, 검증이 시간이 소모되는 단순작업이면서도 중요한 작업에 속하는 불편함을 가지고 있는데, 이를 해결해주는 것이 좋았다.

물론 도커 관련 글에서 언급한 것 처럼, 이미 Jenkins와 같은 CI 도구 등에서 사용해야 할 빌드 명령을 알고 있다면, Dockerfile을 작성할 순 있지만, 안정적으로 자동 생성되는 Dockerfile은 시간과 노력을 아껴주는 편의기능이어서 아주 맘에 들었다.

.NET CORE가 아니더라도, Docker 지원에 대한 CLI 도구들은 넘쳐나게 된 실정이며, CLI로 빌드 가능한 대다수 프로젝트들은, 종속성 관리가 단순하면 단순할수록 쉽게 Dockerfile 작성, Docker 이미지만으로 가동에 쉽게 성공할 수 있다.

---

물리 머신이라도, 가상 머신이라도 네트웍은 어렵디 어려운 부분이고, 비용 문제도 크게 엮인 부분이니 학습이 필요함은 불가피하다.

반면 쉽게 이득을 가져갈 핵심 포인트는 자원 활용에 있는데, **경량 컨테이너의 활용의 정점은 자원 활용의 극대화**라고 할 수 있다.

빠르게 사용량이 비례하여 자원 할당을 함으로써, 서버 비용 감소/리소스 효율 증대를 노리는 것인데, VM보다 훨씬 가볍게 배포/가동/중지가 가능하다는 점은 현재 기준에선 매우 합리적이며, 효율적인 운용 환경을 구축해주는 (쿠버네티스를 비롯한)오케스트레이션 도구 덕분이라고 해도 무방하다.

AWS나, Azure, GCP 등의 메이저 클라우드 서비스들은 당연하게도 지원하고 있다.

* [AWS](https://aws.amazon.com/ko/kubernetes/)
* [Azure](https://azure.microsoft.com/ko-kr/services/kubernetes-service/)
* [GCP](https://cloud.google.com/kubernetes-engine/?hl=ko)

비용 관련 비교는 아래 글이 유용했다.

* [The Ultimate Kubernetes Cost Guide: AWS vs GCP vs Azure vs Digital Ocean](https://www.replex.io/blog/the-ultimate-kubernetes-cost-guide-aws-vs-gce-vs-azure-vs-digital-ocean)

Azure 기준 동일 사양으로 VM 대비 60% 정도의 가격으로 운용할 수 있는데다가, 클러스터 관리 도구 자체를 무료 제공해주므로, 쿠버네티스와 도커에 익숙해졌다면 굳이 더 비싼 VM을 고집할 필요가 없다.

도커 관련 글에서도 언급한대로, 경량 컨테이너다보니 VM을 유지하기 위한 코스트가 온전히 내 어플리케이션이 활용할 수 있는 효율마저 높아진다.

과연 아직도 VM을 고집해야 할까?

---

아직 고민중에 있다면 회사에 남는 머신 두어개 받아서 쿠버네티스를 세팅해보고 운용해보며 그 장점을 몸소 느끼고, QA 서버, 테스트 서버등부터 옮겨가며 그 장점을 누리는 개발자가 늘었으면 하는 바램에서 정리해보았다.

이 글에서는 개념적인 이야기나 원론적인 이야기를 좀 더 많이했는데, 기술적인 이야기는 좀 더 다음번에 좀 더 자세히 다뤄보도록 하겠다.