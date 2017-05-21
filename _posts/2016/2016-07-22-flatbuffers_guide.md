---
layout: post
title: FlatBuffers Guide
date: 2016-07-22 11:14:54
categories: 서버
tags: 서버
comments: true
---

# [FlatBuffers Documentation](https://google.github.io/flatbuffers/)

flatbuffers는 효율적인 크로스 플랫폼 직렬화 라이브러리이다.
C++, C#, C, Go, Java, JavaScript, PHP, Python을 지원한다.

이건 원래 구글이 게임 개발과 또다른 성능이 치명적인 프로그램을 위해 만들어졌다.

이것은 아파치 라이센스 v2 기반의 github 오픈소스로써 사용 가능하다.

왜 Flatbuffers를 쓰는가?
parsing이나, unpacking 전에도, 직렬화 데이터에 접근 가능하다.

### FlatBuffer의 특장점

* 메모리가 효율이 좋고, 빠르다.
* 유연하다.
* 경량 코드
* 강한 타입 처리
* 편리한 사용
* 종속성 없는 크로스 플랫폼 코드

<https://google.github.io/flatbuffers/flatbuffers_guide_writing_schema.html>

## writing a schema

C계열 언어 사용자들과 또 다른 언어 사용자들에게 또한 친숙한 스키마 언어 문법으로 작성된다.

    // example IDL file

    namespace MyGame;

    attribute "priority";

    enum Color : byte { Red = 1, Green, Blue }

    union Any { Monster, Weapon, Pickup }

    struct Vec3 {
     x:float;
     y:float;
     z:float;
    }

    table Monster {
     pos:Vec3;
     mana:short = 150;
     hp:short = 100;
     name:string;
     friendly:bool = false (deprecated, priority: 1);
     inventory:[ubyte];
     color:Color = Blue;
     test:Any;
    }

    root_type Monster;

(Weapon & Pickup not defined as part of this example)
Tables
테이블은 flatbuffers의 오브젝트를 정의하는 주된 방법이고, 이름과 필드 목록을 가지고 있다.

각 필드는 이름, 타입, 선택 사항으로 기본 값을 가진다.

각 필드의 선택 사항 : 이건 반드시 표현 할 필요는 없다, 그리고 당신은 개별 필드 누락을 선택 할 수 있다.

결과 같이, 당신은 유연하게 데이터의 팽창을 두려워하지 않고, 필드를 추가해도 된다.

이 디자인은 flatbuffers의 메커니즘의 전방위적, 배경적 호환성을 위함 이기도 하다.

Note that:
새 필드는 스키마의 테이블 정의 끝에만 추가할 수 있다. 오래된 데이터는 아직 정확히 읽을 수 있고, 그리고 당신은 읽을 때의 기본 값을 줄 수 있다.
오래된 코드는 단순하게 새 필드를 무시 할 수 있다.
만일 당신이 유연성을 원한다면, 특정 순서 보장을 원한다면, 수동으로 대입 (protbuf와 매우 유사), id 속성 아래를 봐라.
당신은 최신 스키마로부터 필드를 삭제를 할 수 없다, 그러나 단순하게 당신의 그 곳에 데이터를 기록하는 걸 멈추는 것과 거의 비슷한 효과를 낼 수 있다. 위의 예제를 보면 부가적으로 당신은 그 곳에 deprecated  마크를 할 수 있고,  C++로 생성 되어 있던 접근자들로부터  보호 될 거고, 필드가 더 이상 사용되지 않는 것으로 시행하는 방법이다. (조심해 : 이건 깨질 수 있는 코드야!)
이름이 변경 된 것들과 원래와 같은 때 까지 코드가 깨지는지 확인한다면, 필드 이름과 테이블 이름을 바꿀 수 있어.

밑에 스키마 진화 예제를 확인하자~

Structs
테이블과 유사하고, 오직 현재 비어있는 필드는 선택적이고 (그래서 디폴트 없음 쪽이나), 그리고 필드는 추가하지 못하거나, deprecated일 것이다.
Structs는 오직 수치형 자료나 또 다른 Structs만을 포함 할 수 있다.

단순한 자료형이나, 매우 확정 적으로 영원히 안 바뀔 것들을 만들 때 써라. ( 꽤 분명한 예는 Vec3)

Structs는 table보다 적은 메모리를 사용하고, 빠른 접근도 가능하다. (그것은 항상 그들의 부모 오브젝트에서 in-line되고, 가상 테이블을 사용하지 않는다)

### Types

크기|내장 수치 타입
---|---
8bit|byte, ubyte, bool
16bit|short, ushort
32bit|int, uint, float
64bit|long, ulong, double


내장 비-수치 타입
다른 타입의 Vector, 계층화 된 벡터는 지원하지 않고, 대신 당신은 테이블안에 내부 벡터로 감쌀 수 있다.
string, UTF-8이나 7-bit ASCII로 고정되어 있다. 다른 텍스트 인코딩 또는 일반 바이너리 데이터는 벡터([byte] or [ubyte])로 대체 할 수 있다.
또다른 tables나 structs, enums, unions 참고해라. (밑을 보시오)

필드 타입은 한번 사용되고 나면 바꿀 수 없다, 예외적으로 같은 사이즈 데이터에 reinterpret_cast를 사용하면 원하는 결과를 얻을 수 있다, 예를 들어 현재 사용되고 있던 최상위 비트 값이 없는 경우 uint를 int로 바꿀 수 있다.

<https://google.github.io/flatbuffers/md__cpp_usage.html>