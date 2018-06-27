---
layout: post
title: 웹 로그 서버 구축기 with rails
date: 2013-04-12 11:14:54
categories: [Ruby, Rails]
tags: [Ruby, Rails]
comments: true
---
현재 웹 로그 서버로 루비 온 레일즈를 사용해 개발중에 있습니다.

그 과정에서 간략한 기록 겸 정리로 포스팅해봅니다.


윈도우용 루비 개발에 이용한 Quick Installer 입니다.

[RailsInstaller](http://railsinstaller.org/)


루비 적응에 참고한 튜토리얼입니다.

[Ruby on rails - getting started Tutorial](http://rubykr.github.com/rails_guides/getting_started.html)

[Ruby on Rails - Active Records Query Interface](http://rubykr.github.io/rails_guides/active_record_querying.html)


scaffold를 이용해 쉽게 기본 기능을 만들어내는 구문입니다.
    
    rails generate scaffold Post name:string title:string content:text


각종 책과 튜토리얼 등을 보면서, 튜토리얼을 보면서도 이해가 잘 안가서 가장 많이 시간을 소요한 부분...
method 등록하는 법에 대한 포스트입니다.

[routing error -> No route matches](https://groups.google.com/forum/#!msg/rubykr/nfryLxkh9oI/bQ4w9lRjafgJ)

요악하자면, 


~~~ rb
# routes.rb
    resources :컨트롤러명 do 
      collection do 
       get :액션 이름
      end
    end
~~~

이렇게 추가해준 뒤,

rake routes를 해주면 등록됩니다.


루비 온 레일즈 사용해본 결과 MVC 모델로 설계 되어있어 역할 분담이 잘 되어있더군요.

**model** : 사용될 데이터 구조 (모델)
**view** : **controller**에서 제어한 동작을 보여주는 코드 정의 
**controller** : **action** 별로 **controller**에 동작을 정의

아직 많이 사용해보지 못했지만, 개발 속도는 압권입니다.
**routing error**에서 헤멘 것 제외하면, 실제 개발 시간은 2시간 남짓에 웹 로그 서버를 구축했습니다. (운용 가능할 정도의 다량의 로그를 쌓는 작업에 대한 측정이나, 테스트는 제외하고 단순 기능 구현)

루비를 서버 제어 스크립트로 작업하고, 이번에 웹 로그 서버 작업을 하면서도 느끼는데, 루비에 대한 호감도가 계속 올라가네요.

자세한 내용은 공개하기 어렵지만, 간략하게라도 삽질기나 팁같은거 공유해보도록 하겠습니다. 