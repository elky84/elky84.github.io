---
layout: post
title: Google이 C++ 호환 언어 Carbon 출시
date: 2022-07-27 00:00:00
categories: [Carbon]
tags: [Carbon, C++, Google]
comments: true
---

## 소개
C++의 실험적 후계자라는 이름으로 어필하고 있다.

### 특징
* 빠르고 C++에서 작동
    * LLVM을 이용하기 때문이며, C++ 코드와 상호 작용이 가능한 수준이라고 한다.
    * 기존 C++ 빌드 시스템에서 작동하는 빠르고 확장 가능한 빌드를 만들 수 있다고 한다
* 현대적이고, 진화함
    * 배우고 쉽고 견고한 언어적 기초를 가지고 있다, 특히 C++ 사용자일 수록 그렇다.
    * 카본 버전 간의 간편한 툴 기반 업그레이드
    * 보다 안전한 기능들, 메모리 안정성
* 오픈 소스 커뮤니티 지원
    * 강력한 주도를 통한 명확한 목표와 우선 순위
    * 열려있고, 포용적이며, 친절한 커뮤니티
    * 컴파일러, 라이브러리, 문서, 도구, 패키지 관리자 등을 지원함

### Carbon을 만드는 이유

* 개발자에게 필수적인 속성인 C++와 일치하는 성능
* C++과의 원활한 양방향 상호 운용성으로 기존 C++ 스택의 어느 곳에서나 라이브러리가 나머지를 포팅하지 않고도 Carbon을 채택 할 수 있습니다.
* C++ 개발자를위한 합리적인 친숙 함을 갖춘 부드러운 학습 곡선.
* 기존 소프트웨어의 설계 및 아키텍처에 대한 비교 가능한 표현력과 지원.
* 관용적인 C++ 코드에 대한 일정 수준의 소스 간 번역을 통해 확장 가능한 마이그레이션

결국 C++의 단점을 개선하고, 장점은 최대한 유지하며, 호환성을 높인 상위 슈퍼셋 언어가 되길 원하고 있다.

이러한 접근을 기반으로, 아래 언어들과 같은 포지션을 지향한다고 볼 수 있고, 실제 github에서도 그렇게 설명하고 있다.

### Carbon이 지향하는 언어적 포지션
* Java Script -> Type Script
* Java -> Kotlin
* C++ -> *Carbon*

### 언어의 목표

* 성능이 중요한 소프트웨어
* 소프트웨어 및 언어 진화
* 읽고, 이해하고, 쓰기 쉬운 코드
* 실용적인 안전 및 테스트 메커니즘
* 빠르고 확장 가능한 개발
* 최신 OS 플랫폼, 하드웨어 아키텍처 및 환경
* 기존 C++ 코드와의 상호 운용성 및 마이그레이션

### C++ 언어와 Carbon 언어의 비교
![c++ snippet](/img/2022/cpp_snippet.svg)
위 코드를 카본으로 옮기면 다음과 같습니다

![Carbon snippet](/img/2022/carbon_snippet.svg)

아래 코드처럼 혼용해서 사용 가능합니다
![Mixed snippet](/img/2022/mixed_snippet.svg)

### 소감

C++을 오래 사용해온 입장에서 설명을 듣자마자 크게 혹해서 실제 사용해보려했는데, 아직 환경 구성도 어렵고 실제 사용은 시간이 좀 걸릴 것 같다.

그럼에도 크게 기대하는 언어이며, C++의 아쉬운 점을 잘 극복해주고, 장점은 더 가진 훌륭한 언어가 되어주었으면 바램이다.


## 공식 자료
* [Google brands Carbon language as “experimental successor to C++”](https://devclass.com/2022/07/20/google-brands-carbon-language-as-experimental-successor-to-c/)
* [Carbon Github](https://github.com/carbon-language/carbon-lang)
## 참고 자료
* [Carbon의 가장 흥미로운 피쳐는 호출 규칙(Calling Convention)](https://news.hada.io/topic?id=7084&utm_source=slack&utm_medium=bot&utm_campaign=TDJ651VS4)