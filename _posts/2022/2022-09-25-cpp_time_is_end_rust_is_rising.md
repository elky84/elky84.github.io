---
layout: post
title: C++의 시대는 저물고 있고, Rust는 떠오르고 있다
date: 2022-09-25 00:00:00
categories: [프로그래밍 언어]
tags: [C++, Rust]
comments: true
---

# 개요

최근 C/C++을 Legacy, Deprecated화 하자는 의견이 다양하게 나오고 있다.

심지어 그 주장이 C++의 발전의 양대 축의 하나인 Microsoft진영의 Azure CTO의 의견이라는 것이 슬프다.

- [Azure CTO: "이제 새 프로젝트를 C/C++로 시작하는 것은 중단할 때가 되었음" - GeekNews (hada.io)](https://news.hada.io/topic?id=7447&utm_source=slack&utm_medium=bot&utm_campaign=TDJ651VS4)

물론 지금은 자바, C#, Kotlin과 같은 매니지드 언어가 더 익숙하지만, 경력의 반 이상을 C++을 사용해온 나로썬 슬픈 감정은 어쩔 수 없는 것 같다.

# Rust의 등장

사실 Go도 C++의 단점을 극복하고자 등장한 언어이며, 꽤 많은 회사에서 사용 중이다.

하지만 Rust는 Go가 일으켰던 반향보다 훨씬 큰 느낌이다.

- [cloudflare가 nginx를 걷어내고 Rust로 HTTP Proxy(Pingora)를 만들어서 사용하다 - GeekNews (hada.io)](https://news.hada.io/topic?id=7400&utm_source=slack&utm_medium=bot&utm_campaign=TDJ651VS4)
- [Programming languages endorsed for server-side use at Meta](https://engineering.fb.com/2022/07/27/developer-tools/programming-languages-endorsed-for-server-side-use-at-meta/)

Meta (Facebook)은 공식 서버 사이드 언어로 선정했으며, cloudfare는 nginx 대신 rust로 HTTP Proxy를 작성했다.

이외에도 수많은 회사에서 성능이 중요한 서버 사이드 언어로 선택하고, 운용하고 있다.

- [Linus Torvalds: Rust will go into Linux 6.1](https://www.zdnet.com/article/linus-torvalds-rust-will-go-into-linux-6-1/)

- [GCC Rust Certificate](https://gcc.gnu.org/pipermail/gcc/2022-July/239057.html)

이와 GCC Rust는 7월에 GCC 운영 위원회로부터 인증 받았으며, 더불어 linux 6.1 커널에 기본 탑재될 예정이다.

앞으로 더 많은 회사와 개발자에게 선택될 일만 남았다는 생각이 든다.

# 과연 한국에선?

사실 잘 모르겠다. 한국은 다른 언어 유행의 흐름을 덜 탔던 편이며, C++은 대체 불가하므로 나름의 소수 분야에서 영향력이 있었지만, 서버 사이드 언어로는 자바가 굳건하다.

현재 Kotlin이 왕좌를 물려받는 느낌이 있지만, 이는 자바를 하위 호환으로 지원하며, JVM 기반 언어라는 점, Spring Boot가 지원된다는 점 등이 영향이 컸다고 생각한다.

그리고 사실상 절대 다수의 웹 서버/서버 사이드 언어로 Spring Boot/Kotlin or Java가 더 오래 인기를 끌 것이라고 예상한다.

그렇지만 상황에 따라 성능 지향 어플리케이션을 작성해야 할 때, Rust가 선택지로 여겨지면서 일부 포지션에서 최우선 선택지가 될 수도 있지 않을까 라는 기대는 갖고 있다.

# 마치며

Rust는 C++의 메모리 안정성/보안 이슈를 상대적으로 많이 극복한 언어이다.

그렇다고 마법은 아니며, 분명히 배우고 익숙해지기에 난도가 높은 편이라고 보는게 옳다.

그럼에도 언어의 규칙만 익힌다면, 성능과 메모리 안정성에서 뛰어난 효율을 낼 수 있는 언어라는 측면에서, 또한 위에서 언급한 수많은 회사에서 사용중이다보니 언어적으로도, 오픈 소스 패키지도 훨씬 더 많아 질 거라는 측면에서 시간을 내 배워볼만 한 언어라고 생각한다.

물론 본인이 시스템 프로그래밍/성능 지향 어플리케이션에 관심이 있을 때 좀 더 유의미할 거라는 생각은 들지만 말이다.