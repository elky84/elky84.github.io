---
layout: post
title: Ubuntu 22.04에 .NET 6 공식 탑재
date: 2022-08-18 00:00:00
categories: [C#]
tags: [C#, .NET 6, Ubuntu]
comments: true
---

### 공식 문서
[.NET 6 is now in Ubuntu 22.04](https://devblogs.microsoft.com/dotnet/dotnet-6-is-now-in-ubuntu-2204/)

요약하자면, .NET 6가 Ubuntu 22.04에 포함되었다는 것이다.

아시다시피 캐노니컬과 MS의 협력은 꽤 오래된 편이며, WSL2를 이용해서 윈도우에서의 우분투 동작은 이미 검증되었다.

상호 협력의 방향이 우분투까지도 넘어간 모양새로 볼 수 있을 것이다.

### 활용 방안

[dotnet-script](https://github.com/filipw/dotnet-script)가 bash shell의 기능 일부를 대신 해 줄 수 있을까?

파이썬과 루비의 운명을 가른 원인 중 하나가 python 2.x 버전의 linux 내장도 영향이 컸고, 이로인해 bash shell로 해야 될 작업을 편하게 지원하는 작업(예를 들어 [fabric](https://www.fabfile.org/))중 다수가 python에서 이뤄졌고, 이에 대한 패키지도 많았음이 이에 대한 반증일 것이다.

우분투까지만 포함된 것이지만, .NET 6 사용자가 많아진다면 다른 리눅스 배포 버전에서도 선택 가능한 부분이라, 점차 사용자가 많아 질 수 있는 계기가 될 수 있을 것 같아 기대가 된다.

---
### MAUI

나도 [.NET MAUI](https://docs.microsoft.com/en-us/dotnet/maui/what-is-maui)를 기대하는 입장이었는데, K 리그 프로그래머님께서 아쉽다는 글[.net maui 로 앱을 만들어볼까 했다가](https://jeho.page/essay/2022/08/19/maui.html)을 쓰시긴했지만, 내 개인적 관점에선 충분히 쓸만한 녀석이긴하다. (물론 Swift에 비해서 아쉽다는건 철저히 공감한다) 

다만 한가지 반론은 

>정식 버전이 나왔는데도 이러면 앞으로도 크게 좋아질 가능성은 없어보입니다.

에 대해서는,

>.NET CORE의 경우에도 3.0이 되었을 때야 만족스러운 개발자 경험을 줬으며, 5.0, 6.0 부터야 확실히 연관 패키지나 주요 패키지들도 성숙했음을 느꼈기에, 꽤 오랜 시간 기다리면 나아지긴 할 가능성도 있다는 생각이다.

---

### 마무리

다시 본론으로 돌아와 C#과 .NET 6를 업무적으로도 많이 쓰고 있는 입장에서, 이러한 발전이 아주 기대가 된다.