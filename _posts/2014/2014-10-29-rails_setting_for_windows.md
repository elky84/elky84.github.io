---
layout: post
title: Ruby on rails 세팅 for Windows
date: 2014-10-29 11:14:54
categories: [Rails]
tags: [Rails]
comments: true
---

[Rails installer](http://railsinstaller.org/en)

![rails_1](/img/2014/rails_1.png)

위 링크에서 windows용 installer를 다운 받아 설치합니다. ruby도 함께 설치되고, 환경 변수도 설정해줘 아주 편합니다.

[Ruby on rails IDE - Aptana Studio 3](http://www.aptana.com/products/studio3/download)

태생이 Eclipse긴하지만, plugin 추가 설치 없이 ruby와 rails 모두를 잘 지원해줍니다.

![rails_2](/img/2014/rails_2.png)

위 두개를 설치 하신 후, 


gem install ruby-debug-ide

명령을 통해 ruby-debug-ide를 설치해주셔야 정상적으로 디버깅이 가능해집니다.



ruby 파일의 경우 run (Ctrl+F11), debug (F11) 명령을 이용하시면 되고, rails의 경우 Run server (Ctrl+Shift+\)나, debug server (not assigned hot key) 를 통해 디버깅이 가능합니다.

![rails_3](/img/2014/rails_3.png)


브레이크 포인트 설정은 해당 라인의 좌측 바에서 더블클릭 하시거나, Ctrl+Shift+B로 가능합니다.

이렇게 세팅하시면 기본적인 윈도우 상의 루비 개발 환경은 완료됩니다.
