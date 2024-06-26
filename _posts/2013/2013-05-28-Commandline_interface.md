---
layout: post
title: 명령행 프로그램 이야기
date: 2013-05-28 00:23:54
categories: [주절주절]
tags: [CommandLine Interface, CLI, 명령행]
comments: true
---

내가 처음 접한 프로그래밍 언어는 `Basic`이 아닌, `C`였다.

그리고 `Turbo-C 2.0`이 첫 컴파일러였다.

내가 처음 샀던 `C언어` 서적이 `Turbo C 2.0`을 알려주는 주황색 서적이었는데, 뭔가 시리즈 였던 기억이 난다.

그 책이 너무 설명이 어려워, 다음에 샀던 책이 바로, [Turbo-C 2.0 길라잡이](http://books.google.co.kr/books/about/%ED%84%B0%EB%B3%B4_C_2_0_%EA%B8%B8%EB%9D%BC%EC%9E%A1%EC%9D%B4_S_W%ED%8F%AC%ED%95%A8.html?id=9MZ3MgAACAAJ&redir_esc=y)다.

내 기억에 이 책의 표지는 초록색이었는데, 사진도 목차도 안나와 있어서 정확히 이 책이 맞는지는 확신하긴 어렵긴하다.

여튼 당시 내가 봤던 서적에서의 `"Hello, World!"`는 다음과 같다.


~~~ cpp
#include < stdio.h >
void main()
{
    printf("Hello, World!\n");
}
~~~

뭐....당시엔 대다수 국내 C언어 서적이 저랬을려나...?
ANSI C Programming의 번역서만이.


~~~ cpp
#include < stdio.h >
int main(int argc, char* argv[])
{
    printf("Hello, World!\n");
    return 0;
}
~~~

였던걸로 기억한다.

고작 이게 뭐 그리 중요하냐고?
이 별거 아닌 차이가, 유닉스 문화와 윈도우 문화를 가르는 발단이 되었기 때문이다.

GCC에서는 애초에 `void main()`이 허용되지 않는다.

유닉스에서는 명령행 프로그램 사이의 연동에 인자`(int argc, char* argv[])`를, 넘기고 그 결과로 `exit 코드 (main의 int 리턴 타입)`를 이용한다.
그래서 프로그램간의 연동이 쉽게 가능하며, 명령행 프로그램 위에 UI를 붙이는 과정이 이런 전제하에 있다. (물론, 파이프 통신이나 소켓 통신등으로 IPC를 하기도 하지만)


여러 포스팅을 보면, `void main()`과 `int main(int argc, char* argv[])`의 차이에 대해 표준에 대한 이유를 언급하곤 하는데, 이 표준의 전제에는 프로그램 간의 상호 작용을 매끄럽게 하기 위해서라 할 수 있다.

`터보-C`와 `MSC`또는 `MSVC`라 불리우는 `Visual C++`이 `void main()` 을 허용하다보니, 그렇게 만들어진 프로그램의 `exit코드`는 신용할 수 없다.

물론 리눅스 환경에서 개발됐고, ANSI C 표준을 따른 프로그램이라해도, 목표로한 동작을 완료하지 못했음에도 `exit코드`를 0이 아닌 값을 반환하는 상황도 있을 수 있으나, 대부분의 규칙을 잘 따른 프로그램들은 정상 수행에 0을 반환한다는 것을 목표로 삼고 있다.

이 반환값을 기반으로 도는 프로그램은 수없이 많다.

---

나는 철저히 윈도우 프로그래머였다. 그것도 클라이언트 온리 게임만 만들던 동인 게임 개발자였고.

애초에 프로그램간의 연동이 목표가 아니라, 그저 내 프로그램 하나가 수많은 기능을 담고 싶어했던, 바퀴를 다시 발명하는 것을 즐겼던(?) 그저 흔한 프로그래머였음은 물론이고. 심지어 게임 개발할 때마다 전용 툴까지도 만들었던... 흑역사를 품고 있다.

내가 윈도우 쉘 연동으로 생산성을 높이게됐던 계기는, 서버 개발자가 된 후 라이브팀 업무때가 주로 그랬다. 나에게 주어진 시간은 그다지 많지 않았고, 컨텐츠를 개발하는 일 이외의 시간은 사실 잘 주어지지 않았다. 그렇다보니 자연스레 바퀴를 재발명하는건 시간이 부족하다보니 여의치않아졌다.

가능하면 기존에 있는 기능들을 잘 묶고, 명령행 프로그램 여러개의 기능을 엮는 자동화에 대한 의지가 늘게된 시기였다. 

물론 이전에도 난 게으르기 때문에, 게을러 지기 위한 기반 작업. 자동화에 대한 의지가 있었으나 그 자동화를 `C++` 코드위에서 하곤 했다. `FTP`고, `HTTP`고 `WIN32 API`를 통해 다시 만들어, 내 프로그램 위에 얹었다.

잘 만들어져 있는 명령행 프로그램들을 엮는 일이, 얼마나 내 생산성을 높아지게 했는지 뼈저리게 느꼈다.

그와 함께 기존에 안좋았던 습관들이 조금씩 사라지게 된 계기가 됐고.

---

그런 깨닳음을 얻어가던 중 내가 만든 여러 프로그램들을 돌이켜봤다.


어째서 내 프로그램은 어찌 하나같이, `GUI` 기반으로만 동작하는가?!?
`GUI`기반으로 동작하는건 좋다 이거야. 헌데, 다른 프로그램과 엮기 왜 이렇게 힘든거지?

내가 짠 프로그램들은 하나같이 `main함수`에서 `return 0`으로 프로그램을 종료시키고 있었다.
원하는 목표를 달성했는지 여부와는 전혀 상관없이 말이다.

기능을 재사용하는건  라이브러리화 해둔 코드를 재사용하는 것에 의존했을 뿐이었고.



별거 아닌거 같으면서, 큰 깨닳음을 얻고, 자동화와 각종 보조 업무등을 위해 다른 사람들이 만든 프로그램들을 엮는 작업이 늘어가면서 내가 만든 프로그램들도 그 사이에 엮을 수 있게끔  맞춰가고 있다.


아직 많이 미숙하지만 리눅스 환경에서 ROR을 운영하며 리눅스에서의 기능 재사용, 프로그램 사이의 연동의 문화, 흔히 리눅스 문화라 불리는 것들을 잘 느낄 수 있었다. 

윈도우 환경 위에서 만들어진 수 많은 메이저 프로그램이 명령행 기능을 지원하지 않는다. 

특정 프로그램에서 95%가 만족스러운데, 살짝 아쉬운 한두가지 요소 때문에, 다른 프로그램을 찾아 다녀야했던 일이 리눅스 문화라고 없는건 아니지만, 명령행 위에 UI를 얹은 많은 프로그램들은 이런 고민을 해결해준다.


이런 깨닳음이 늦게 온 원인이 윈도우 환경 위에서 프로그래밍을 해왔기 때문이라고 핑계대고 싶진 않다.
다만 `GUI` 환경의 문화가 프로그램 사이의 연동을 어렵게 만드는 요인이라는 생각을 갖게 됐다.

아직 능숙해지려면 멀었지만 리눅스 환경에 조금씩 적응될수록 여러가지 생각이 드는데, 그 중 명령행 프로그램에 대한 이야기는 꼭 한번 하고 싶었다.

앞으로도 윈도우 환경에 대한 감사함을 얼마나 느끼게 될지, 리눅스 환경에 대한 감탄을 얼마나 하게 될지는 잘 모르겠지만 윈도우'만' 써왔다고 할 수 있는 프로그래머가, 리눅스 환경에 적응해가며 드는 감상을 적어보겠다. 