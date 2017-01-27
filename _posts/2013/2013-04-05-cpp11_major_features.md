---
layout: post
title: C++ 11 주요 Feature 정리
date: 2013-04-05 11:14:54
categories: C++11
comments: true
---

[Ten C++11 Features Every C++ Developer Should Use](http://www.codeproject.com/Articles/570638/Ten-Cplusplus11-Features-Every-Cplusplus-Developer)

위 article을 간략하게 요약해봤습니다.

1. Auto
Auto는 C++ 0x에서도 주요 Feature였죠.
컴파일 타임 타입 유추 기능입니다.
일반적으로 typedef 해서 자료형을 정의해두고, iterator, value_type 등을 사용해야 했던 번거로움을 한번에 날릴 수 있는 좋은 기능이죠.

2. nullptr
NULL을 대체하는 type 타입입니다.
NULL이 ((void*)0)이나 0으로 define 되어있는 점을 감안 했을 때, type 검사가 이루어질 수 있는 nullptr은 모호함을 제거하기에 적합하다고 보여지네요.

3. range-based for loops
for each와 같은 느낌의 범위 전체 순환 기능입니다.
for each ( obj in data_structure ) 와 동일한 기능으로 보여지네요.
추가적인 장점이라면 배열에도 사용 가능해졌다는 점입니다.

4. override and final
override는 가상함수를 override했음을 의미하고, 상위 클래스중에 overriding될 메소드가 없으면 에러를 뱉는 키워드인데요, 이 키워드가 표준화임을 의미하는군요.
final은 override를 더이상 할 수 없게 막는 키워드죠.

5. Strongly-typed enums
enum을 강화해줬네요. 기존의 enum은 상수 나열형으로 유용하긴하지만, 다른 타입들보다 사용시 어려운 점이 많아 힘들었다는 점을 감안 했을 때, 아주 강해졌다고 보여집니다.

6. Smart pointers
개념 및 life-cycle의 이해를 위해 unique_ptr (구 auto_ptr), shared_ptr (흔히 smart_ptr을 shared_ptr만을 지칭하는 걸로 오해하곤 하는데 아닙니다.), weak_ptr을 적절히 사용하면 코드 가독성을 높일 수 있습니다.
이 세가지 smart pointer 에 대한 설명이네요.

7. Lambdas
익명 함수의 유용성은 더 말해 입이 아플 정도겠죠?

8. non-member begin() and end()
반복자를 member외에 추가로 template method화 했다는 의미입니다.
일관성을 위해 변경했다고 하고... 배열에도 사용가능해졌다는 장점도 있다고 하네요.

9. static_assert
GPG 3권에서 나왔던 compile_assert와 유사한 기능이 표준화되었군요.
컴파일 타입 검사를 강화하는 기능입니다.

10. Move semantics
R-value reference의 효율을 의미하는 것으로, 동일한 객체의 리소스를 다른 객체로 전송해서, 복사 비용을 줄이는 매너니즘을 의미합니다.
실제로 이로 인한 성능 향상을 c++ 0x에서도 많이 봤었죠.
정확히 하자면 move semantics의 구현중 r-value reference가 있는 개념이라고 하네요.

이렇듯 C++이 진화하고 있습니다.
루비같은 언어와는 애초에 접근자체가 다르긴하지만, 자바나 기타 C계열 파생 언어등에 비해 갖고 있던 리스크를 지속적으로 해결하고 있는 걸 보면 뿌듯하네요.
서버 개발은 node.js 등으로 많이 옮겨가고 있지만, 아직 C++은 죽지 않았다고 생각합니다. (개인적으론 앞으로도 계속 유지될 언어라고 생각합니다. 왠만한 언어들보다 라이프 사이클이 더 길거라고 믿는 편이죠)

여기까지 C++ 11에 대한 간략한 정리였습니다. 감사합니다.

[C++ 11 Wiki](http://ko.wikipedia.org/wiki/C%2B%2B11)