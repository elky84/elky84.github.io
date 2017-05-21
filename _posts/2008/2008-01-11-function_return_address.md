---
layout: post
title: 함수의 리턴 값의 주소를 사용하려 할 때 생기는 문제
date: 2008-01-11 11:14:54
categories: C++
tags: C++
comments: true
---

    char* GetStr()
    {
        static char szStr[] = "Hello";
        return szStr;
    }

    void PrintStr(char **str)
    {
        printf("%s",*str);
    }


    int main(int argc, char **arv)
    {
        PrintStr(&GetStr());
        return 0;
    }
 
위 코드는 아래와 같은 컴파일 에러를 발생시킨다.
    error C2102: '&' requires l-value

컴파일러의 에러는, l-value. 즉, 어딘가에 저장된 값에만 주소 연산자를 사용할 수 있다는 말이다.

내가 이 코드를 작성한 의도는 char형 포인터의 포인터 (이중 포인터)를 매개 변수로 받는 PrintStr함수의 매개변수로, char형 포인터를 반환하는 GetStr함수의 반환 값의 주소를 넘기면 정상적으로 동작할 거라는 생각에서였다.

그래서 내가 생각한 해답은, GetStr함수가 반환할 char형 포인터 주소인

    GetStr() 0x00427b60 "Hello"           char *

가 아니라, GetStr의 반환 값이 임시 저장되어 있는, 레지스터의 주소를 대입하려 하는 건 아닐까 생각했다. 

 
main함수를 이렇게 바꾸면 정상적으로 동작한다.

    int main(int argc, char **arv)
    {
        char *str = GetStr();
        PrintStr(&str);
        return 0;
    }

이 코드를 디스 어셈블 해본 결과다.

    char *str = GetStr();

    00412BDE  call        GetStr (411500h) 
    00412BE3  mov         dword ptr [str],eax

역시 추측이 맞았다. GetStr함수를 부른 결과는 eax 레지스터에 저장되어 있었고, 그 값을 어딘가에 복사하기 전까진, 레지스터에 있는 값이므로, 레지스터에 주소 연산자를 사용할 수 없기 때문에 컴파일 에러를 낸 것이었다. 
