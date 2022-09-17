---
layout: post
title: 도구의 중요성
date: 2018-06-01 17:08:00
categories: [개발 도구]
tags: [개발 도구]
comments: true
---

>명필은 붓을 가리지 않는다.

이 말이 와전 된 말이라는 것은, 이젠 꽤나 많은 사람들이 알게 됐다.

>'명필이 붙을 안가린다'라지만 사실은 붓을 가린다. - <http://yopangyopang.tistory.com/296>

개발 도구도 똑같다.

툴마다 어마어마하게 큰 생산성 차이를 낸다.
중요한 것은 절대적 툴 차원에서의 지원의 차이도 물론 존재하지만 자신에게 익숙한 툴인가도 매우 중요하다. (특히 단축키)

너무나 당연한 얘기지만 중요하게 여기는 포인트는, 내가 원하는 기능들이 온전히 제공 되는가이다.

intellisense, open declaration, open implement, formatting, checkstyle, debugging (watch, stacktrace, step into, step out, recompile and continue 등), performance, package manager interlocked 같은 다양한 관점에서 얼마나 만족도가 높은가이다.

**다양한 관점의 만족도가  전반적 유료 툴들이 높은편이지만, 비용은 현실적인 문제니 검토 대상에 포함될 수 밖에 없다.**

나는 Visual Studio를 오래 써왔었고, 자바를 처음 사용할 때도 IntelliJ IDEA를 써서 못 느꼈는데 Eclipse는 큰 확장성에 비해서 너무 느렸다.

![Visual Studio](/img/2018/logo_vs.jpg)
    
<blockquote>
Visual Studio는 완벽에 가까운 개발 툴이다. 
현재는 Community 버전으로 개인 프로젝트 단위는 무료 사용 가능하다는 큰 장점이 있다.
</blockquote>

 ![Intellij IDEA](/img/2018/logo_ij.jpg) 
 
<blockquote>
JetBrains의 명성을 널리 알리기 시작한 시발점이 된 Java IDE.
Key Set과 Color Theme만 바꿔도 딱히 손댈 게 없을 만큼 기능 지원이 우수하다.
</blockquote>


 ![Eclipse](/img/2018/logo_eclipse.jpg)

<blockquote>
오랜 기간 명맥을 유지해온 무료 IDE. 
Java 전용이 아님에도 Java 진영에서의 오랜 선택을 받아온지라 그런 이미지가 있다.
Plugin 설치로 기능 확장이 여러가지 면에서 가능하나 대다수의 기능 설정을 개인이 해야하며, 성능 이슈가 존재하는 아쉬움이 있다.
</blockquote>


Eclipse를 사용하고 있던 팀에 합류했다보니, 나도 자연스레 Eclipse로 환경 설정을 하게 됐고, 
기능적으로는 부족함이 없었음에도, 플러그인으로 구현되면서 문제로 보여지는 많은 파일을 잡고 있는 문제 (runtime이 아닐 때에도 pom.xml을 통해 참조되는 maven jar를 모두 잡고 있었고, UI 갱신 속도도 무척이나 느렸다. 아마도 보안 프로그램과의 충돌이 원인인 상황)였는데, 테스트 삼아 설치해본 IntelliJ IDEA의 trial버전은 이런 문제가 전혀 없었다.

그리고 그것이 IntelliJ IDEA 구매 요청하게 된 계기가 됐다.

---

보안 프로그램과의 충돌의 가능성이 높은 것은, 파일 억세스를 통한 기능 확보 방식으로 구현된 기능들이 문제였다.

이는 쉽게 개선되기 어려운 문제이기도하고, 무료 툴에서 덜 관심을 가지는 성능 이슈에 대한 아쉬움이기도 했다.

그에 비해 Eclipse와 같은 무료툴이며, 범용 툴인 Atom이나 VS Code는 plugin 설치로 인한 실행 속도 감소가 매우 적었다.
다만 플러그인을 통한 기능 지원도 제한적이긴 했다.

![Atom](/img/2018/logo_atom.jpg)

<blockquote>
Github에서 만든 Text Editor이자, IDE.
Plugin 방식으로 기능이 확장 되는 구조다.
</blockquote>

![Visual Studio Code](/img/2018/logo_vscode.png)

<blockquote>
최근 내가 사용하고 있는 툴. Atom 기반이나 단축키가 Visual Studio 키셋과 동일하며, 
Plguin 개발이 활발해 매우 다양한 언어 개발 용도나, 
Text Editor 용도로써의 사용에도 매우 편리하다.
</blockquote>
    
가볍게 사용 했을 때는 문제가 없었지만, 특정 언어나 프레임워크에 종속적인 설정 파일 관리/편집 기능, linter, formatter 설정에서 아쉬움이 존재했다.

Notepad++같은 순수 Text Editor보다는 구동 시간이 느리지만, IDE들 보다는 훨씬 빠른 구동 시간은 장점이다.

요약하자면, 자신이 툴 종속적인 기능을 잘 활용하고, 그에 대한 메리트를 느낀다면 전용툴, 유료 툴을 적극 검토해야 한다는 의미다.

---

내 경우에는 사용해야 하는 툴의 종류로 IDE, Database Tool, SSH Tool, REST API Test Tool, Text Editor 등의 툴들이 필요하다.

여기에 사용해야 되는 프로그래밍 언어, DB 종류가 늘어난다면 사용해야 될 툴이 늘어날 수도 있다.

사용해야 되는 툴이 늘어난다면, 툴의 특화 기능도, 단축키도, 암묵적인 룰도 학습 해야하며, 태스크 전환시 부담이 될 수 있다.

**도구가 내 손에 딱 맞았을 때의 생산성은 매우 높다. 반면 도구에 익숙하지 않은 상태에선 한번 더 생각하거나, 어떤 메뉴나 단축키로 설정된 기능인지 생각해야 될 때의 생산성은 매우 낮다.**

내 경우에는 Visual Studio로 개발을 시작했으므로, 여러 개의 툴을 사용할 시에도 Visual Studio key Set으로 모두 맞추어서 적응 코스트를 낮춘다.

내가 필요한 기능이 해당 툴 선정 조건에 포함되었으므로, 당연히 툴 사용에 불편함은 없다.

이와 관련해 나는 **집, 노트북, 회사 PC의 환경을 최대한 일치시키려 하는데, 이는 작업시의 적응 비용을 최소화 하기 위한 노력**이다.

애초에 환경을 최대한 유사하게 세팅 해둔다면, 불시에 작업을 해야 되거나, 머릿속의 생각을 구현하기 위해 바로 코딩을 하려 했을 때 환경 구성에 대한 시간 소요 없이 즉시 작업에 돌입 할 수 있기 때문이다.

**자신이 최상의 퍼포먼스를 내는 환경이 무엇인지 떠올려보고, 그런 환경에 최대한 근접하게 하기 위한 노력이 필요하고, 그 중에서도 내 손에 맞는 도구는 매우 중요도가 높다.**

자신이 효율성에 대해 고민하는 사람이라면, 도구의 중요성도 고민 선상에 두는 것은 어떨까?