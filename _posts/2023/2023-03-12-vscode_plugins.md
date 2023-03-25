---
layout: post
title: VS Code Extensions 추천
date: 2023-03-12 00:00:00
categories: [VS Code]
tags: 
- Extensions
- IDE
- VS Code
comments: true
---

최근에는 Visual Studio나 JetBrains 제품군이 아니라, VS Code로 개발하는 사람들이 많아졌다.

워낙 Extensions를 잘 쓰면 굳이 상용 IDE가 없이도 수월한 개발이 가능하기 때문일 것이다.

물론 Kotlin을 쓰면 Intelli IDEA가 더 좋고, C#을 쓰면 Visual Studio가 더 좋은 것은 사실이긴한데, 가벼운 개발이나 가벼운 기능 개발이나 이슈 확인, 단순 증상 디버깅과 같은 가벼운 용도로는 충분히 쓸만하다.

특히나 나는 노트북 여러대, 데스크탑까지 쓰는데 이럴 경우 IDE를 너무 많은 종류를 쓰면 환경 구성이 귀찮기 때문이기도 하다. (물론 한번만 세팅하면 되긴하지만... Visual Studio는 용량부터 크다)

또 언어에 종속적이지 않은 IDE 세팅을 공유할 수 있다는 것도 큰 장점이라고 볼 수 있다.

그리하야, 언어에 비 종속적이거나 덜 종속적인 플러그인 추천을 해보겠다.

## [Paste Image - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image)

![](/img/2023/vscode_paste_image.png)

![paste image](https://raw.githubusercontent.com/mushanshitiancai/vscode-paste-image/master/res/vscode-paste-image.gif)

이미지 복사 붙여넣기 편의성 제공 플러그인.

본인의 스타일에 따라서 세팅해서 쓰면 아주 편리하게 쓸 수 있음.

Github Pages등으로 마크다운 글쓰기를 한다면 꼭! 깔아야 할 확장!

## [Docker - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

Docker 사용 현황과 연동되어 조금 더 편하게 관리할 수 있다.

![](/img/2023/vscode_docker.png)

![](https://github.com/microsoft/vscode-docker/raw/HEAD/resources/readme/overview.gif)

## [Kubernetes - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools)

마찬가지로 Kubernetes를 시각화하고 관리하기 편하게 해준다.

![](/img/2023/vscode_k8s.png)



## [IntelliCode - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode)

![](/img/2023/vscode_intellicode.png)

![intelli code](https://aka.ms/IntelliCodeUsageExamplesv2)

Code Assistant 확장

Python, TypeScript/JavaScript, Java 개발자에게 도움을 준다.

 

## [Code Runner - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)

대다수의 개발 언어를 실행 시켜주는 도구

지원하는 언어는 너무 많아서 대부분 된다고 볼 수 있는 수준

![](/img/2023/vscode_code_runner.png)

![Code Runner](https://github.com/formulahendry/vscode-code-runner/raw/HEAD/images/usage.gif)



## [Indentation Level Movement - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=kaiwood.indentation-level-movement)

Indent 기준으로 Windows, Linux는 Ctrl + up, down, Mac은 Alt + up, down 으로 가능하다.

![](/img/2023/vscode_indentation_level_movement.png)

![Indentation Level Movement](https://github.com/kaiwood/vscode-indentation-level-movement/raw/master/images/indentation-level-movement.gif)



## [Bracket Peek - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=jomeinaster.bracket-peek)

Bracket이 어디에 속해있는지를 알려준다.

긴 함수나 Bracket이 긴 코드를 안만들면야 좋겠지만... Flutter나 Java Script 등 Frontend 개발시에는 종종 그렇게 하는게 나을 때도 있으니 유용함

이 역시 다양한 언어를 지원함

![](/img/2023/vscode_bracket_peek.png)

![Bracket Peek](https://raw.githubusercontent.com/j0meinaster/bracket-peek/master/assets/preview.gif)



## [Surround - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=yatki.vscode-surround)

![](/img/2023/vscode_surround.png)

코드를 래핑해주는 기능을 제공해주는 확장입니다.

기존 코드 복사하고, 구문 작성하고 다시 붙여넣는 과정을 생략할 수 있다.

![](https://raw.githubusercontent.com/yatki/vscode-surround/master/images/demo.gif)

![](https://raw.githubusercontent.com/yatki/vscode-surround/master/images/demo2.gif)



## [Better Comments - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)

주석의 타입에 따라 다른 색으로 표기해서, 작성자의 의도를 읽는 사람에게 잘 전달해주는 확장

이 규칙을 그대로만 따라해도 꽤 쓸만하다

![](/img/2023/vscode_better_comments.png)

![](https://github.com/aaron-bond/better-comments/raw/HEAD/images/better-comments.PNG)



## [Log File Highlighter - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=emilast.LogFileHighlighter)

로그 파일이 컬러로 하이라이팅 된다.

로컬 로그를 VS Code로 viewer로 쓸 때 좋다

![](/img/2023/vscode_log_file_highlighter.png)

![](https://raw.githubusercontent.com/emilast/vscode-logfile-highlighter/master/content/sample.png)



## [MongoDB for VS Code - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)

전용 편집기 만큼 편하게 쓸 수는 없지만, 간단히 데이터를 확인하고, PlayGround를 통해 각종 쿼리를 날리는 정도는 활용 가능하다.

개인적으로 [NoSQLBooster for MongoDB - 엘키의 주절 주절](https://elky84.github.io/2022/01/25/NoSQLBooster/)을 쓰는게 더 다방면에서 좋긴하지만, 서브 PC에서 가볍게는 충분히 쓸만했다.

![](/img/2023/vscode_mongodb.png)



## [PostgreSQL - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ckolkman.vscode-postgres)

MongoDB와 마찬가지로, 세부적인 기능은 부족하지만, 가볍게 쓰기엔 나쁘지 않다.

당연히 디테일한 기능은 [pgAdmin - PostgreSQL Tools](https://www.pgadmin.org/) 을 쓰는 것이 좋다.

![](/img/2023/vscode_pgsql.png)



## [Excel Viewer - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=GrapeCity.gc-excelviewer)

Excel 파일을 수정해야 할 때 가볍게 쓸만하다.

약간 반응이 느릿한 면도 있고, 디테일한 기능들 중 불가능 한 것도 많지만 가볍게 쓰기엔 충분히 괜찮았다.

![](/img/2023/vscode_excel_viewer.png)

![](https://github.com/jjuback/gc-excelviewer/raw/HEAD/img/csv-preview-4.gif)



## [Remote - SSH - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)

SSH를 가볍게 쓸 때 쓰면 유용하다.

생각보다 사용성이 괜찮긴 하지만, 베스트하다고 보긴 어려움.

특히 VS Code 화면 레이아웃이 크다보니 SSH로 전문적 작업을 할 거면, 전문 툴을 쓰거나 Terminal 어플리케이션으로 쓰는 것이 더 좋긴 하다.

![](/img/2023/vscode_remote_ssh.png)

![](https://microsoft.github.io/vscode-remote-release/images/ssh-readme.gif)



## [REST Client - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

REST API 테스트용 샘플을 작성하고 관리하기에 매우 유용하다

settings.json을 통해서 환경 변수까지 더해지면, 각종 환경에서 유연하게 테스트 시나리오를 사용할 수 있다.

후술할 Thunder Client 대비 Plain Text 기반이므로, 시나리오를 복붙하거나 편집 속도가 빠른 장점이 있다.

![](/img/2023/vscode_restclient.png)

![](https://raw.githubusercontent.com/Huachao/vscode-restclient/master/images/usage.gif)



## [Thunder Client - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=rangav.vscode-thunder-client)

REST API 테스트 세트를 UI로 관리하고 사용하는게 더 편하다면 Thunder Client가 더 좋다.

Collections로 그룹화 해서 관리하기도 좋고, Env를 통해 환경 변수 관리도 시각화되서 조금 더 유리하다.

Postman이 익숙하다면 더더욱 REST Client 대비 장점이 많다.

![](/img/2023/vscode_thunder_client.png)

설정을 공유한다면, Save To Workspace 옵션을 켜서 json 파일을 Workspace에서 관리하도록 하고 써야 한다.

![](/img/2023/vscode_thunder_client2.png)

![](https://github.com/rangav/thunder-client-support/blob/master/images/thunder-client-v2.png?raw=true)



## [Error Lens - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)

감지된 진단에 대해서 시각화 강화해준다.

실제로 조금 더 눈에 들어와서 잘 체크하게 도와주는 좋은 기능.

![](/img/2023/vscode_error_lens.png)

![](https://raw.githubusercontent.com/usernamehw/vscode-error-lens/master/img/demo.png)