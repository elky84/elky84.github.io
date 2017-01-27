---
layout: post
title: Ruby on rails 소개
date: 2014-12-02 11:14:54
categories: Rails
comments: true
---

![rails_1](/images/rails_1.jpg)

Rails는 Ruby로 작성된 MVC모델을 기반 프레임워크입니다.
 
쉽고 빠르게 웹 어플리케이션을 구축 할 수 있도록 도와줍니다.
 
Rails와 비슷한 역할을 하는 프레임워크로는 java spring, python django, php code igniter 등이 있습니다.
 
Rails의 장점은 Ruby 자체가 가진 유연성이라고 볼 수 있습니다.
Rails 자체도 Ruby로 이루어졌지만, Rails application도 ruby로 작성됩니다.
 
Ruby의 모든 것은 객체이며 동적 타입을 통한 코드 유연성을 기반으로 한 손쉬운 회귀 테스트 등 장점이 무궁무진합니다.
 
제가 느꼈던 Rails의 장점을 나열해보겠습니다.
 
Ruby로 프로그램을 작성한다.
이는 생각보다 큰 장점입니다. 루비 언어 자체가 생산성이 좋은 언어입니다.
Ruby는 메소드 접미사로 !나, ?를 통해 역할의 의미를 함축해 포함하기도 하며, 메타 프로그래밍에도 능수 능란한 언어입니다.
MVC모델의 완성도가 높다.
모델명에 맞추어, 동일한 이름의 View, Control만 구현하면 무리 없이 동작합니다.
또한 Rake를 통해 MVC 모델의 각 구성요소를 쉽게 build할 수 있고, scaffold 옵션을 통해 MVC 구성 요소를 한번에 구축해주기도 합니다.
라이브러리 연동이 편하다. (리눅스에서는)
안타깝게도 윈도우에서는 미동작하는 gem이 좀 많습니다.
만약 리눅스 환경이라면, 웹 서비스에 까지 필요한 대부분의 기능은 잘 동작한다고 보셔도 무방합니다.
리눅스 환경의 단점으로 일컬어 지는 라이브러리 버전간 호환성 문제는 gemfile에서 특정 버전까지 명시함으로써 우회 가능합니다.
 
제 개인적으로는 윈도우 환경에서 서비스까지 하기엔 적절치 않습니다만, 리눅스 서버 환경에서 관리한다는 전제하에선 생산성 최고의 언어가 아닌가 생각합니다.
 
단점으로 일컬어 지는 속도 문제는, ruby는 느리지만 rails는 빠르다 라는 말도 있듯이, 충분히 php에 근접한 속도를 낼 수 있습니다. 이에 대해선 링크로 대신하겠습니다.
<http://www.comentum.com/ruby-on-rails-vs-php-comparison.html>
<http://stackoverflow.com/questions/2529852/why-do-people-say-that-ruby-is-slow>
 
 
새로 시작할 웹 개발용 프레임워크를 고민 중이시라면, Rails 어떠신가요?
빌드 업 속도를 Rails(철도길)를 달리듯 빠르게 도와줄 겁니다.

![rails_2](/images/rails_2.jpg)

[Ruby on Rails 공식 페이지](http://rubyonrails.org/)
[RAILS 시작하기](http://rubykr.github.io/rails_guides/getting_started.html)
[REST란?](http://www.joinc.co.kr/modules/moniwiki/wiki.php/man/12/rest/about#s-2.1)
