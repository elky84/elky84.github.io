---
layout: post
title: 능률적인 프로그래머
date: 2017-08-09 00:15:00
categories: [서평]
tags: [자기관리]
comments: true
---

Code Complete, 실용주의 프로그래머 같은 부류의 프로그래밍 실천 지침서의 일종이다.

그와 함께 내가 늘 주장하는 **좋은 이야기는 많이 들어 지겨워질 정도라면 정말 반드시 지켜야 할 가치가 있는 것들** 이라는 맥락에서 가치 있는 책중 하나다.

혹시나 이 서평을 읽는 분 중, 이 책이 궁금해져서 찾게 되실까봐 미리 언급하자면, 이 책보다 실용주의 프로그래머나 Code Complete가 더 좋다.

책의 페이지 수가 많지 않음에도 불구하고, 장황하고 정돈이 안된 책 이란 생각이 들었다. (실용주의 프로그래머와 비교 했기에 더욱 그랬을 지도 모르겠다.)

특히 사례에 근거하려 했던것 같지만, 조금 날카로움과 디테일에서 아쉬움도 분명히 있다.

이런 책들은 시간이 흐를 수록 그 가치나 의미가 조금씩 쇠퇴하는 경우가 많은데, 그런 주제나 내용도 꽤 담고 있다.

챕터의 사례가 디테일을 다루려다 핵심을 잘못 짚는 느낌이 있었기 때문이다.

개인적으론 회사 동료분께 받은 책이라 짬을 내서 읽게 됐는데, 책이 얇다보니 금방 읽을 수 있다는 점을 제외한다면 굳이 찾아서 읽을 정도는 아니란 생각이 든다. 

누누히 언급한대로 대체제가 워낙에 막강하고 현재 시점에서 이 책보다 훨씬 더 유효하기 때문이다.

---

주요 내용 요약
---
### 가속
* 단축키 잘 써라 
* 클립 보드 잘 활용하라
* 매크로 잘 써라

### 집중
* 산만 요소 제거
	* 소리
		* 헤드폰 끼자
	* 시각
		* 불필요 알림 disable
* 검색 기능 잘 써라
* 루티드 뷰로 작업 폴더에만 집중하라
* 프로젝트 주요 폴더 숏컷 활용하라
	* 윈도우의 경우 바로가기 메뉴 잘 활용하면 될 듯
* 다중 모니터 사용하라
* 가상 데스크톱 활용하라

### 자동화
* 바퀴 다시 안만들기
	* SVN, Git
	* CruiseControl, Jenkins
	* Trac, Mantis
	* Migle, Redmine
* RSS 피드 연동
* Rake, Ant, 비빌드 등의 make 유틸리티를 업무 용도로 전환
* 쉘 스크립트를 활용하라
* 스크립트 언어로 반복 작업을 스크립팅하라
	* 수작업으로 하는게 더 빨랐을 지 몰라도, 단순 반복 작업을 손수하다보면 멍청해지고, 가장 능률적 자산인 집중을 놓치게 된다.
* 자동화 정당성 증명
	* 단위 테스트
	* 기능 테스트
	* 통합 테스트
* 야크 털깍기로 변질 되지 않는지 경계하라
	1. 서브 버전 로그를 기반으로 문서를 생성하려 한다
	2. 서브 버전 후크를 추가하려다 사용중인 웹서버 버전이 낮아 호환되지 않음을 발견한다
	3. 웹 서버를 업데이트 하려는데 해당 버전은 현재 운영체제 패치 레벨에서는 지원되지 않음을 깨닫고 운영체제 업데이트부터 시작한다
	4. 운영 체제 업데이트는 컴퓨터가 백업을 위해 사용하는 디스크 배열과 알려진 문제가 있다.
	5. 디스크 배열 관련 문제 해결을 위해 시험적 패치를 내려 받는다. 듣기는 하는데 이번에는 비디오 드라이버가 오동작한다.
	* 야크 털깍기가 위험한 이유는 엄청난 시간이 소비되기 때문이다. 왜 업무소요 시간 예측이 그리 잘 빗나가는지 역시 설명된다.
		* 본질을 해결하기 위한 노력에 들어가는 코스트도 계산하라.

### 정식화
1. DRY 버전 제어
	* 바이너리까지 버전관리 시스템을 사용할땐 너무 커져서 관리 코스트와 비용이 증가함.
		* 이진 파일은 네트워크 시스템에 올리고, 개발 컴퓨터가 참조하는 방식도 생각해보라.
2. 빌드 PC 두기
3. 간접 참조
	* Maven, gradle, nuget, npm, gem, pip 등등의 패키지 시스템을 말함
4. 가상화 사용
	* VMWare, VirtualBox등을 통한 Vagrant 등으로 환경 재현을 의미
5. DRY 교류 저항 불일치
	* 데이터 매핑
		* Hibernate, Ibatis 등의 OR 매퍼 사용시 세 군데에 반복 저장
			1) 스키마, XML 매핑 문서, 클래스 파일
		* 문제 해결을 위해 원본을 하나만 두고 이를 기반으로 산출 해야 함
			* 스키마 기반 코드 생성기
				* Rake Migration의 경우 정의를 기반으로 스키마, 모델 파일등을 생성하므로 불일치가 발생하지 않는다.
				* dbDeploy를 이용하면 버전별 차이와 롤백 기능 등을 지원한다.
6. DRY 문서화
	* 유효하지 않은 문서는 잘못된 정보를 퍼트릴 위험이 있다.
	* SVN2Wiki
		* SVN 저장 내용 기반의 자동화 문서이므로, 살아 있는 문서가 된다.
	* 클래스 다이어그램
		* 가능한 모든 기술 문서를 자동 생성하자
		* 같은 사본을 절대 둘 이상 두지 말자.
		* Doxygen 등을 이용해서 문서화 하라
	* 데이터베이스 스키마
		* 스키마 기반 코드 생성기 툴을 사용하라

### 테스트 주도 설계
* 단위 테스트는 코드의 정결함을 유지하는 좋은 방법.
1. 진화하는 테스트
	* 유닛 테스트가 이미 결합도를 낮춰놓았으므로 요구 사항이 바뀌거나 추가 되었을 때 대응이 용이하다.
2. 코드 검사

### 정적 분석
1. 바이트 코드 분석
	* 컴파일 된 코드에서 버그 패턴을 읽어낸다.
	* 정확성
		* 가능 버그 표시
	* 나쁜사례
		* 필수 또는 추천 코딩법 위반. (equals() 메소드만 재정의하고 hashCode()는 재정의 하지 않은 경우 등)
	* 교모함
		* 혼란 스러운 코드. 색다르게 사용했거나, 이상하고 서툰 코드.
2. 소스 분석
	* 가능 버그
		* 빈 try...catch 블록 같은
	* 데드 코드
		* 사용하지 않는 지역 변수, 매개 변수, private 변수
	* 부최적 코드
		* 비경제적 문자열 사용
	* 지나치게 복잡한 코드
		* 지나치게 긴 메서드.
		* 너무 많은 멤버를 포함한 클래스.
	* 중복 코드
		* 복사해 붙여넣기로 '재사용'
3. 파놉티 코드로 측정 지표 산출
	* 파놉티 코드 같은 측정 지표 산출 도구를 사용하라
4. 동적 언어 분석
	* 순환 복잡성 (사실상 모든 블록 기반 언어가 공통으로 가진) 및 코드 검사에 집중한다.

### 좋은 시민 의식
1. 캡슐화 깨기
	* 가능하면 열지 말자
	* 그래도 열어야 하면, 메소드로 열자
	* 단순 프로퍼티 (getter, setter)를 확인하는 테스트 코드는 사용하지 말자.
2. 생성자
	* 특정 형식의 유효한 객체가 되기 위해 필요한 요소가 뭔지 알려주는 역할이다.
3. 정적 메소드
	* 블랙박스, 독립 실행형 상태 비저장 메소드.
	* 고정성 (staticness)와 상태 저장 (state)를 혼동하면 큰 문제가 발생한다.
4. 범죄 행위
	* 메소드를 너무 작게 분리하면 실패인지 판가름 하기 힘든 오동작을 더 막기 쉽다.
		* 예를 들어 월과 일을 따로 메소드로 호출하는 java.util.Calendar는 2월 31일 입력 시, 3월 2일로 변환해버린다.
			* 이를 한 메소드로 통합시에는 예외를 발생시키면 된다.

### YAGNI
* 쓸데 없을 걸이란 뜻
* 오버 엔지니어링 하지 말고 필요한 기능을 잘 만들고 완성도를 높이는 데에 집중하라는 뜻.

### 고대 철학자들
1. 아리스토텔레시의 본질과 비본질 속성
	* 본질적 복잡성
		* 풀고자 하는 문제의 핵심이라 어려워도 용서가 된다.
	* 비본질적 복잡성
		* Gorufcorrhjk 직접 관계가 없으면서 어쨌든 풀어야 하는 모든 문제를 뜻한다.
2. 오캄의 면도날
	* 여러 가설이 있을 경우 가장 간단한 게 정답일 확률이 높다는 주장이다.
	* 맨먼스 미신
		* 지체되는 개발 프로젝트에 인력을 더하는 건 개발을 늦출 뿐이라는 주장.
3. 디미터의 법칙
	* 가장 친한 친구 몇 명하고만 소통할 것
		1. 개체 자신의 메소드
		2. 매개 변수로 넘어온 개체의 메소드
		3. 메소드 내 생성 개체의 메소드
4. 소프트웨어 전승 지식
	* 고서를 많이 읽자
		1. 맨먼스 미신
		2. 실용주의 프로그래머
		3. 벡의 스몰토크 최상의 방법 패턴
		4. 기타 등등

### 권위에 도전하기
1. 성난 원숭이 떼
	* 항상 그렇게 해왔으므로 그렇게 하는 것이 많다는 얘기.
		* 항상 그렇게 해왔으니까요는 충분한 이유가 되지 못한다
		* 왜 항상 그리 해왔는지 이해한다면 이유가 되고, 그렇다면 반드시 계속하자.
2. 유창한 인터페이스
	* 도메인 특정 언어 (DSL) 양식 유형의 하나
	* 하나의 완전한 생각을 길게 연속되는 코드로 엮어 구어체로 유리화 시키듯 문장을 만든다.
3. 반 객체
	* 객체 및 객체 계층 구조가 대부분의 문제를 위한 훌륭한 추상화 기법을 제공해 주는 반면, 바로 그 때문에 더 복잡해지기도 한다는 요지다.
	* 팩맨의 예
		* 명백한 해결책
			* 유령에게 지능을 부여
		* 간단한 해결책
			* 미로에 지능을 넣는 쪽
		* 이것이 반 객체식 접근.

### 메타 프로그래밍
1. 자바와 리플렉션
    * 리플렉션으로 메소드를 호출할 경우, 훨씬 더 지능적인 팩토리 클래스 작성 및 런타임 클래스 로딩이 가능.
    * 이를 이용하면 대다수 플러그인 아키텍처에서 구체적 클래스 없이도 컴파일 타임에 특정 인터페이스를 준수하는 뭔가를 새로 작성 할 수 있다.
2. 루비의 extend 활용
    * 더 짧은 코드를 위해 기존 클래스의 메소드나 동작 확장

### 컴포즈드 메소드와 SLAP
* 컴포즈드 메소드
    * 모든 실제 절차는 private 메소드에서 구현
        * 외부에서 접근해야 되는 경우 public메소드가 private 메소드를 호출
    * 코드를 더 작고, 응집력 있고, 읽기 쉽게 분해하도록 장려함.
* SLAP
    * 메소드 내의 모든 코드가 같은 추상화 수준을 가져야 함을 뜻한다.
    * 저수준 데이터 베이스 연결을 하면서, 고수준 비즈니스 코드나 다른 웹서비스 직통 연결이 들어가서는 안된다는 얘기.

### 다종 언어 프로그래밍
* 컴퓨터 언어는 정체하면 죽어버리는 상어와 같다.
* 범용 언어 외에 하나 이상의 특수 목적 언어를 사용한 어플리케이션 개발을 뜻함.
* 올라의 피라미드
    * 안정 > 비공식 >  DSL순의 피라미드
    * 목적들을 위한 다른 언어로 개발된 것을 제어하는 DSL을 만들고 관리하는 것을 말함.

### 안성 맞춤 도구 찾기
* 개발 목적에 최적화된 IDE를 찾아라
* Cygwin, ubuntu bash, mingw 등을 잘 활용하라

### 팀 생산성을 위한 패턴
* 콘웨이의 법칙
    * 하나의 컴파일러를 만들기 위해 4개의 팀을 만든다면, 4개의 컴파일러를 얻게 될 것.
* 땅콩 버터와 마천루
    * 기능이 중심이 되어 소프트웨어를 만드는 Bottom-Up 방식의 프로세스
    * 마일 스톤 형태와 같이 초기 단계에서는 땅콩 버터 방식으로 인프라를 구축하고, 인프라 구축 이후에는 다음과 같이 한다.
        * 땅콩 버터 방식을 유지하되, 릴리즈가 거듭 될 때 마다 더불어 사용 가능한 시나리오 들을 점진적으로 증가시켜 테스트 하는 방식이 좋은 예가 될 것이다.
* 적재 적소에 팀원 배치하기
    * 개인의 작업 방식, 기술 선호도 등을 발견하고, 잘 고려하여 구축하고자 하는 시스템의 구조적인 레이어나 기술에 할당하는 것이 매우 중요하다.
* 분할 후 정복 (높은 생산성을 유지하라!)
    * 장기적인 목표를 단기적인 목표로 나누어 팀원들에게 목적 의식과 방향성을 제시해야 한다.
    * 그리고 개발자들은 이 마일스톤에 집중해서 문제를 완벽히 이해하고 해결하기 위한 문제 공간을 만들어, 거기에 집중할 수 있도록 끊임없이 독려해야 한다.
* 단결력 있는 팀
    * 이러한 문제 공간에 끊임없이 팀원들을 참여시키고 흥미를 가질 수 있게 자극한다면 팀의 응집력은 자연스럽게 증대 될 것이다.
    * 실제 사용자와 팀원들이 대화하게 하라
    * 가끔씩 현장 방문을 통해 팀원들이 시스템이 가지고 있는 도메인 특성이나 문화를 경험하게 하라
    * 잠자고 있는 도멩니 전문가를 찾아서 대화하라
    * 문제 공간에서 중요한 결정 사항 등을 검증하고 공유하라
* 숨어 있는 잠재력에 주목해라.
    * 결과물에도 다양한 측면에서 여러 가지 행위 들에 적정한 가중치를 두어 팀의 진척도를 측정해야 한다.
    * 잠재력을 끌어내기 위해서는 피그말리온 효과를 이용하는 것이 좋다
* 인수인계는 이렇게
    * 떠나는 개발자가 미완료한 일, 버그, 문서 작업을 빨리 진행 할 수 있도록 다른 일을 시키면 안된다.
    * 인수인계는 새로운 사람보다는 같이 일을 해온 동료나 팀원에게 분배하는 것이 좋다.
    * 틈틈이 문서와 산출물을 점검하고, 주기적인 지식 공유를 할 수 있게 해야 한다.
* 공간 확보의 중요성
    * 문제에 완벽히 집중하기 위해서는 15분의 시간이 필요하다.
    * 방해 요소를 제거하기 위해 공간 확보가 되면 좋다.
