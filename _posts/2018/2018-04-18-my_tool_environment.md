---
layout: post
title: 나의 개발 환경 (심플 ver)
date: 2018-04-18 00:00:00
categories: [개발 환경]
tags: [개발 환경]
comments: true
---

간단히 제가 개발 중에 사용하는 세팅을 표로 정리해보았습니다.

좀 더 상세하게 적으려면 표 형식이 잘 안어울려서, 간략한 표로 우선 보여드리고 별도의 글로 분리 작성하겠습니다. (특히 이클립스가 세팅이 훨씬 복잡)

쓰고보니 VS Code plugin 소개 같은 글이 되어버렸네요. 궁금하신 점이나 좀 더 상세히 알고 싶으신 것 있으면 disqus로 댓글 부탁드립니다.

|용도|프로그램명|설치 URL|플러그인|테마|
|:---:|:---:|:---:|:---:|:---:|
|Visual Studio Code 세팅 동기화|Visual Studio Code|https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync, [언어 설정](http://igotit.tistory.com/entry/Visual-Studio-Code-UI-%EC%96%B8%EC%96%B4-%EB%B3%80%EA%B2%BD)||Monokai|
|RDB|heidisql [pgsql, mysql (&mariadb), mssql 지원]|https://www.heidisql.com/|||
|MongoDB|mongoui|https://github.com/azat-co/mongoui|||
|Git|TortoiseGit|https://tortoisegit.org/|||
|SVN|TortoiseSVN|http://tortoisesvn.tigris.org/|||
|C++, C#|Visual Studio 2017|https://www.visualstudio.com/ko-kr/products/visual-studio-community-vs.aspx|[ProductivityPowerTools](https://jacking75.github.io/VS_Productivity_Power_Tools/)|Monokai|
|.net core|Visual Studio Code|https://www.microsoft.com/net/download/windows|https://marketplace.visualstudio.com/items?itemName=doggy8088.netcore-extension-pack|||
|Java, Python, C++ (Cygwin), Ruby|Eclipse Oxygen [용도별로 반드시, workspace를 따로 써야 한다. 특히 플러그인 종류가 다른 프로젝트들이 같이 쓰면 매우 느려짐]|http://www.eclipse.org/downloads/|Dev Style - Aka Darkest Theme, [Ansi Escape in Consol](https://marketplace.eclipse.org/content/ansi-escape-console), [CheckStyle](http://lahuman.jabsiri.co.kr/158), https://www.lesstif.com/pages/viewpage.action?pageId=39126240, [Lombok](https://projectlombok.org/)|[Sublime Text Monokai Extend](https://github.com/eclipse-color-theme/eclipse-color-theme/blob/master/com.github.eclipsecolortheme/themes/sublime-text-monokai-extended.xml)
|RESTClient|Visual Studio Code||https://marketplace.visualstudio.com/items?itemName=humao.rest-client||
|RESTClient|Postman|https://www.getpostman.com/||
|Markdown|Visual Studio Code||https://marketplace.visualstudio.com/items?itemName=hnw.vscode-auto-open-markdown-preview||
|Python|Visual Studio Code|https://www.python.org/downloads/|https://marketplace.visualstudio.com/items?itemName=donjayamanne.python||
|Node.js|Visual Studio Code|https://nodejs.org/en/|https://marketplace.visualstudio.com/items?itemName=leizongmin.node-module-intellisense||
|Go|Visual Studio Code|http://golang.org/dl|https://marketplace.visualstudio.com/items?itemName=lukehoban.Go||
|Ruby|Visual Studio Code|http://railsinstaller.org/en|https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby||
|PlantUML|Visual Studio Code|https://graphviz.gitlab.io/download/|https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml||
|Rust|Visual Studio Code|https://www.rustup.rs/|https://marketplace.visualstudio.com/items?itemName=saviorisdead.RustyCode||
|Documentation|Doxygen|http://www.stack.nl/~dimitri/doxygen/download.html|||
|Redis|Redis|https://github.com/MicrosoftArchive/redis|https://redisdesktop.com/download||
|Terminal|WinSSHTerm|<https://winsshterm.blogspot.kr/>, Preferences/Terminal/Terminal Type : putty-256 color, Send TCP Keepalive to remote system : Yes, every 30 seconds 이렇게 해야 안 끊기고 세션 유지 됨.|||