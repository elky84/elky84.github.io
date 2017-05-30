---
layout: post
title: C++ 구조체 이니셜라이저 문제
date: 2013-06-14 11:14:54
categories: C++
tags: C++
comments: true
---

    struct CHAR_COLLECTION_DATA
    {
        int CharID;
        int Value;
        int ValueCode;
    };

    CHAR_COLLECTION_DATA CollectionData  = {m_CharID, m_Value, m_ValueCode };

이런 코드가 있었다.


기능을 추가 하시려다보니 습관적으로

    struct CHAR_COLLECTION_DATA
    {
        int CharID;
        int ClassID; // 다른 변수를 중간에 추가함.
        int Value;
        int ValueCode;
    };

    CHAR_COLLECTION_DATA CollectionData  = {m_CharID, m_Value, m_ValueCode };

같은 코드고 컴파일 오류도 없지만 원래 코드와 다르게, CharID, Value, ValueCode를 채우지 않고, CharID, ClassID, Value에만 값을 채우는 코드가 되어버렸다.


물론 사용 코드를 전부다 훓어보지 않은 문제가 있긴 하지만, 컴파일 오류로 강제되지 않은 초기화도 좋은 제약은 아니다.

    struct CHAR_COLLECTION_DATA
    {
        int CharID;
        int ClassID; // 다른 변수를 중간에 추가함.
        int Value;
        int ValueCode;

        CHAR_COLLECTION_DATA()
                    : CharID(0)
            , ClassID(0)
            , Value(0)
            , ValueCode(0)
        {

        }


        CHAR_COLLECTION_DATA(int charID, int classID, int value, int valueCode)
            : CharID(charID)
            , ClassID(classID)
            , Value(value)
            , ValueCode(valueCode)
        {

        }
    };

기존 코드가 이렇게 짜여져 있었다면, 기존 코드였던

    CHAR_COLLECTION_DATA CollectionData(m_CharID, m_Value, m_ValueCode);

는 컴파일 오류를 일으킨다.

자연스레 

    CHAR_COLLECTION_DATA CollectionData(m_CharID, m_ClassID, m_Value, m_ValueCode);

위와 같이 고치게 될 것이다.


물론 모든 상황에서 주의 깊게 코드를 찾아보고 고치면 얼마나 좋겠냐 만은, 커버리지는 높을수록 좋은 것.

컴파일 오류로 막을 수 있는 습관은 갖추는 게 좋다.

구조체 이니셜라이저는 자제 하자.

첨언을 하자면, 대입될 변수를 지정하는 방식의 이니셜라이저는 추천이다. legacy한 문법을 허용하는 C스타일 구조체 이니셜라이저를 자제하자는 얘기다.