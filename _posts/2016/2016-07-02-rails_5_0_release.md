---
layout: post
title: Rails 5.0 Release
date: 2016-07-02 11:14:54
categories: Rails Ruby
comments: true
---

### Rails 5.0: Action Cable, API mode, and so much more
* <http://weblog.rubyonrails.org/2016/6/30/Rails-5-0-final/>

Rails 5.0이 정식 릴리즈 되었습니다.

드디어! 웹소켓을 지원합니다.
Action Cable이 바로 그것이죠.

기존 rails의 구조가 1 request-1 response를 기반으로 하는 만큼, 얼마나 웹소켓의 이벤트와 Rails ActionController 코드와 유연하게 연동이 되는지는 궁금합니다.

벌써 한글로 된 채팅 앱 구현 글이 올라왔네요!
<http://blog.ask.co.de/2016/06/%EB%A0%88%EC%9D%BC%EC%A6%88-5%EC%9D%98-%EC%95%A1%EC%85%98-%EC%BC%80%EC%9D%B4%EB%B8%94%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-%EC%B1%84%ED%8C%85-%EC%95%B1-%EA%B5%AC%ED%98%84/>

Api Mode는 back-end로 client-side javascript나 native-application과 JSON으로 통신이 가능하다고 하네요.

정확히 어느정도로 다른 어플리케이션과 유연하고, 성능상 잇점을 가져다줄 기능이 구현되었는지는 확인해봐야 알거 같네요.


빠른 개발 속도와, 상대적으로 낮은 학습비용에 비해 성능 문제와, 진입 자체는 쉽지만 많은 이해도를 요구하는 난제들도 함께 했던게 사실인 rails.

특히나 성능 문제로 인해 twitter는 rails를 포기하기도 해 많은 우려를 낳고 있었는데요, rails 5는 그런 우려를 불식시키고, 다시금 많은 장점으로 사랑받는 웹 프레임워크로 입지를 확고히할지는 더 두고 봐야할 문제라고 생각합니다.

자세한 Rails 5 차이점에 대한것은 제가 조금 더 사용해보고 알려드리겠습니다.

rails 5에 대한 간략한 사용법과 특징을 정리한 유투브 영상 한편 소개해드리며, 마무리할게요.

<iframe width="560" height="420" src="://www.youtube.com/embed/OaDhY_y8WTo?rel=0" frameborder="0" allowfullscreen=""></iframe>