---
layout: post
title: MobaXterm 소개
date: 2022-01-17 00:00:00
categories: [RDP, SSH, VNC, Tool,Recoomend]
tags: [RDP, SSH, VNC,Tool,Recoomend]
comments: true
---

`RDP`, `SSH` 프로그램은 다양하게 많다.

둘다 지원되는 무료 툴로는 [mRemoteNG](https://mremoteng.org/), [Remote Desktop Manager](https://remotedesktopmanager.com/) 와 같은 툴이 있다.

### mRemoteNG

![mremoteng](/img/2022/mremoteng.png)

### Remote Desktop Manager

![rdm](/img/2022/rdm.png)

둘다 충분히 훌륭한 툴이고, 쓸만한 무료툴이다.

RDP만 생각하면 [Remote Desktop Connection Manager - Windows Sysinternals](https://docs.microsoft.com/en-us/sysinternals/downloads/rdcman) 도 괜찮은 툴이고, SSH만 생각하면 [WinSSHTerm](https://winsshterm.blogspot.com/), [Termius - SSH platform for Mobile and Desktop](https://termius.com/), 같읕 툴도 참 좋은 툴이다.

### WinSSHTerm

![winsshterm](/img/2022/winsshterm.png)

### Terminus

![termius](/img/2022/termius.png)

물론 좀 더 편한 관리 기능이 필요 없다면, `MSTSC`, `putty` 같은 기본 툴을 사용하는 경우도 있으며, 윈도우에서도 이제 `ssh` 기본 탑재라 충분히 커맨드라인으로도 일정 수준 편하게 쓸 수 있다. 

다만 컬러 지정에 제한이나, RDP, SSH, VNC 등을 두루 편하게 쓰기 위해선 툴이 필요한데, 위에 언급된 툴들보다 조금 더 만족스러운 툴이 있어서 소개한다.

[MobaXterm free Xserver and tabbed SSH client for Windows](https://mobaxterm.mobatek.net/)

![mobaxterm](/img/2022/mobaxterm_01.png)

SSH 연결시, 좌측에 보이듯 자동으로 home 경로와 연결 된다.

이를 통해 파일 drag & drop이 손쉽게 가능하다. (전송, 수신 둘다 가능하다)

또한 테마도 다양하며, 메뉴바 크기 조정 옵션도 다양해 좀 더 컴팩트하게도 사용이 가능하다.

RDP의 경우 smart resizing도 아주 아주 편한 편이며, 일하면서 모니터 내에서 좌우이동, 다른 모니터로 이동을 자주 하는편인 나로썬, 이 smart resizing의 깔끔함이 아주 맘에 든다.

### [MobaXterm Xserver with SSH, telnet, RDP, VNC and X11 - Features (mobatek.net)](https://mobaxterm.mobatek.net/features.html)

위 페이지를 보면 다양한 기능에 대한 설명이 있는데, 나열하자면 다음과 같다

1. **Tabbed terminal**
2. **Sessions management**
3. **Graphical SFTP browser**
4. **X11 server**
5. **Enhanced X extensions**
6. **Multi-execution**
    - 같은 명령을 여러 서버로 내리는 건데, 요즘은 오케스트레이션 도구가 발달해 SSH로 직접 연결해 명령을 내리는 일이 흔치는 않지만, 있으면 좋은 기능이긴 하다.
7. **Embedded servers**
8. **Embedded tools**
9. **Remote Unix desktop (XDMCP)**
10. **Remote Windows desktop (RDP)**
11. **SSH gateway**
12. **SSH tunnels (port forwarding)**
13. **MobApt package manager**
14. **Text Editor**
15. **Macros support**
16. **Passwords management**
17. **Syntax highlighting in terminal**
18. **Professional Customizer**

### 지원 프로토콜

![mobaxterm](/img/2022/mobaxterm_02.png)

단점은 무료 버전의 경우 세션 제한이 있다. 유료 버전이 아주 비싸진 않다. 구독형은 아니라 부담이 덜하지만, 12개월 업데이트만 지원되는 점은 조금 아쉽긴하다.

또 윈도우 환경에서만 사용 가능한 것도 아쉬운 요소다.

![mobaxterm](/img/2022/mobaxterm_03.png)

그럼에도 윈도우 환경의 개발자이며, 윈도우 서버, 리눅스 서버를 모두 사용한다거나, 윈도우 업무 PC로 원격 업무를 해야 되는 경우라면 충분히 사용할만한 퀄리티의 좋은 툴이라고 볼 수 있다.

대부분의 경우 위 최대치를 넘기지 않을 것이기에 큰 단점은 아닐 수 있다.

개인적으로 소규모 개발까지의 사용에서 최적화 된 툴이라는 생각에 추천한다.