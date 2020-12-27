---
layout: post
title: C# .NET CORE 기반 Lol-Crawler
date: 2020-12-27 00:00:00
categories: [Crawler, LOL]
tags: [Crrawler, LOL]
comments: true
---

Web-Crawler와 마찬가지로, 자동화의 일환으로 개발했다.

[elky84/lol-crawler: Notification from LOL friend game start & end.](https://github.com/elky84/lol-crawler)

지인 분들과 취미로 롤을 종종하는데, 직접 시간을 맞추지 않고선 지인들이 플레이 하는 중인 것을 확인하기 어려워 개발하게 됐다.

[MingweiSamuel/Camille: C# Riot API Library. Thread safe, automatic retries, autogenerated nightly releases.](https://github.com/MingweiSamuel/Camille)

Riot API를 직접 구현할수도 있었겠지만, 이미 구현된 API Wrapper를 이용해서 구현했다.

원래는 전적 사이트나 데이터 분석용도도 고민했으나, 롤을 띄엄 띄엄 하기도하고, 다른 Dev Toy에 시간을 쏟고 있는 중이라, 친구의 플레이 시작/종료(결과 포함) 알림 기능까지만 구현되어있다.

C# .NET CORE 기반의 Riot API 연동이 궁금하신 분께 도움이 되면 좋겠다는 생각에 Github로 공개한다.