---
layout: post
title: VSCode with Rails
date: 2016-04-14 11:14:54
categories: Rails Ruby VSCode
tags: Rails Ruby VSCode
comments: true
---
현재까지 개발툴로 C#과 C++에서는 이견이 없이 visual studio 2015를 사용해왔습니다.
ruby (& rails) 의 경우 visual studio에서 지원하지 않아, windows환경에선 aptana studio 3 (http://www.aptana.com/)를 사용했고요.

그러던차 node.js를 쓸 일이 좀 있어, visual studio에 node.js 플러그인을 설치해 사용하려 했습니다만… 버그인지 CPU 점유율이 25%이상을 점유하고, 메모리가 계속 증가해 visual studio가 크래시 되더군요. 같은 설치 상태에서, C#, C++은 문제가 없는데 node.js 프로젝트에서만 발생했습니다.
-> VS2015 Update 2 이후 발생하지 않네요.

그리하여 다른 솔루션을 찾던 중, vs code를 사용해보았습니다.
node.js 기반에서 며칠 사용해보니, 여러모로 편리했습니다 . atom기반 이라지만, atom의 학습비용이 드는 환경 설정 문제가 해결되어있어, 더더욱 편리하게 사용 가능했습니다.

node.js 개발 환경으로 아주 만족스러웠습니다. (visual studio 사용자 기준, atom보다 친숙하고, 편하다고 생각합니다) 자연스레 ruby (& rails) 개발 환경으로도 써볼까 하는 욕심이 들었는데요, 

그리하여, vs code의 ruby plugin을 찾기 시작했습니다.
<https://marketplace.visualstudio.com/vscode/Languages?sortBy=Downloads>

몇개 없더군요. 실제로 ruby의 debug plugin은 아래 하나라고 보시면됩니다.
<https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby>

[plugin github 링크](https://github.com/rebornix/vscode-ruby)

설치 방법은 Ctrl + P를 누르시고, ext install Ruby를 입력하시면 됩니다.
그리고 F5를 누르시면, 어떠한 언어로 해당 환경에서 구동 할 건지를 결정하는 창이 뜹니다.

Ruby를 선택하시면 기본 환경 설정 파일 (launch.json)이 생성됩니다.

저는 기본 생성된 json에서, program에서의 디버깅할 rb파일 명 (main.rb -> xlsx_magician.rb)과 args (없으면 ARGV[0]에 string undefined가 환경 변수 ARGV에 담겨 스크립트에 전달 됩니다.)를 추가했습니다.

여기서 주의하실 점은 "args": [""] 로만 비울 경우는 undefined가 아니라, ARGV자체가 not available 상태라는 점입니다. "args" 항목을 아예 사용하지 않았을 때와 달라요.

### launch.json에 "args" 항목 미사용시
- ARGV = not available
### "args": [""]로 넘길 경우
- ARGV = undefined 
### "args": ["Character.xlsx"]
- ARGV = ["Character.xlsx"]


자 이제 rails 디버깅 환경에 대해 살펴볼까요?

rails의 경우에는 launch.json을 보강해주셔야 합니다.
<https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby>을 참고하세요.

그리고 좌측에 DEBUG 구동 설정에서 Debug Local Files로 선택되어 있는 것을, Rails Server로 바꿔주셔야 합니다.

rails가 4.2.x로 넘어오면서, 기본 binding 주소가 local에서만 접근가능하게 바뀌었습니다.
그렇다 보니 환경 변수를 넘기고 싶었는데요, 위 설정에서 args에 아무리 파라미터를 확장해봐도, rdebug-ide.bat 파일에 넘어가는 인자가 확장되더군요. 오픈 소스인지라 github의 해당 소스 코드를 직접 찾아보니 더 명확해지더군요. rdebug-ide.bat에 넘기는 인자만 확장 될 뿐입니다.

그리하여, 기본적으로 -b 0.0.0.0으로 bind 하는 것과 같은 결과를 내기 위한 방법을 찾아봤습니다.

아래 링크에서 보이듯 boot.rb를 고쳐주시면, 기본적인 서버 구동시에도 0.0.0.0으로 bind 됩니다.
<https://fullstacknotes.com/make-rails-4-2-listen-to-all-interface/>

물론 여전히 아쉬운 점은, 다른 인자를 못 넘긴다는 점인데요, 이 부분은 환경 설정 rb 파일을 변경해서 테스트하는 방법 정도로 절충해야 할 거 같네요.

vscode-ruby를 변경하는 방법도 있긴 하지만, 저는 우선 여기까지로 환경 설정을 마무리했습니다.

실제 개발 환경으로 사용하는 데에 크게 무리가 없었습니다. eclipse 기반의 aptana studio 3가 큰 만족도가 아니었다는 점도 한 몫 한다고 보고요.

vscode도 아직은 ruby 개발 환경으로 아쉬운 점이 있지만, 그래도 현재 까지의 윈도우에서의 개발 환경도 그다지 만족스럽지 못한, 번거로운 점이 많았다는 것을 감안했을 때, 대안이 될 수 있지 않을까 합니다.