---
layout: post
title: Meta (Facebook)에서 서버 사이드 언어로 Rust를 승인
date: 2022-08-01 00:00:00
categories: [Rust]
tags: [Rust, Meta, Facebook]
comments: true
---

## 일부 내용 발췌

### What is a supported language at Meta?
Meta’s primary supported server-side languages are Hack, C++, Rust, and Python. 

* 성능에 민감한 백엔드 서비스의 경우 C++ 및 Rust를 권장합니다. Rust는 이 목록에 새로 추가된 것입니다. 우리 제품과 서비스에는 Rust 발자국이 급속히 증가하고 있으며, 우리는 Rust를 장기적으로 약속하고 얼리 어답터를 환영하고 있습니다.
* CLI 도구의 경우 Rust를 사용하는 것이 좋습니다. 이것은 올해의 새로운 권장 사항입니다.
* 비즈니스 로직 및 상대적으로 무국적 애플리케이션의 경우, 해킹 생태계는 메타에서 최고 수준의 자동화 및 지원을 제공하며 권장되는 언어입니다.
* 마지막으로, 메타는 파이썬 개발자들을 계속 많이 지원하고 있습니다. 데이터 과학, ML 애플리케이션 및 Instagram의 경우 Python은 계속해서 선택의 언어이며, 우리는이 생태계에 대한 경험에 계속 투자하고 있습니다.
* 특정 사용 사례의 경우 Java, Erlang, Haskell 및 Go를 포함한 다른 언어를 지원합니다. 이러한 언어는 현재 특정 사용 사례 외부에서 널리 지원되지 않습니다.

### How did we arrive at our list of supported languages? 
* *핵심 라이브러리 지원.*
    * 고립 된 서비스는 거의 없으며 언어가 적을수록 핵심 라이브러리에 대한 부담이 줄어 듭니다.
* *보안 및 개인 정보 보호.*
    * 조각난 스택은 중요한 보안 및 개인 정보 보호 기능을 서비스에 구축하는 복잡성을 증가시킵니다.
* *운영 위험.*
    * 일부 서비스에 심각한 문제가 발생하면 즉각적인 지원이 필요합니다. 우리는 생산 문제를 진단하고 해결하는 데 엄청난 양의 전문 지식을 축적해 왔으며, 사고 대응은 주요 사건에 도움이되는 서비스를 읽고, 이해하고, 디버깅 할 수 있어야합니다. 조각화를 피하면 운영 위험이 줄어듭니다.
* *전문성.* 
    * 우리는 이러한 각 언어에 대한 전문 지식을 갖춘 중요한 엔지니어를 구축하고 유지합니다.
* *개발자 경험.*
    * 지원되는 언어에는 IDE 지원, 빌드 속도, 디버깅 경험 등과 같은 영역을 개선하기 위해 노력하는 팀이 있습니다.

## 요약

한번 결정된 프로그래밍 언어는 쉽게 빼기 어렵고, 비용도 크기 때문에 아주 신중하게 결정했다.

성능에 민감한 백엔드 서비스에서 `C++`과 `Rust`를 권장하는데, 이 카테고리에 Rust가 추가 된 것.
또한 CLI 에서는 압도적으로 `Rust`를 권장하고 있다.

## 소감
개인적으로 `Rust`를 눈여겨 본지 꽤 됐는데, 적극적으로 사용해보지는 못했다.

[Rust 튜토리얼 추리 게임](https://rinthel.github.io/rust-lang-book-ko/ch02-00-guessing-game-tutorial.html) 정도 까지 하고 더 진행을 못했는데, 이번 기회에 조금 더 파고들어 보고자 책 [한 줄 한 줄 짜면서 익히는 러스트 프로그래밍](http://www.yes24.com/Product/Goods/110368348)도 구매했다.

조금 더 해보는 중인데, 확실히 전보다 환경 구성도 편해졌고, 레퍼런스도 많아진 느낌이다. 

물론 한국에서 `Rust`를 사용하는 회사는 많지 않지만, 그럼에도 익숙해지고 싶은 욕심이 드는 언어고, 이제 해외에서는 적극적으로 쓰는 회사가 많아진 것을 느끼게 되서 더욱 그렇다.


### 공식 자료
* [Programming languages endorsed for server-side use at Meta](https://engineering.fb.com/2022/07/27/developer-tools/programming-languages-endorsed-for-server-side-use-at-meta/)