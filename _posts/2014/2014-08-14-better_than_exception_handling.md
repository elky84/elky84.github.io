---
layout: post
title: 코드 작성과 디버깅을 쉽게하는 예외 처리
date: 2014-08-14 11:14:54
categories: [C++, 예외처리, 디버깅]
tags: [C++, 예외처리, 디버깅]
comments: true
---

이 글은, c++을 기반으로 작성되었지만 예외 처리 기능 (try-catch, try-except 등)을 가진 모든 언어에 적용 되는 내용입니다. 파일에서 데이터를 읽는 코드를 작성해봅시다. c++로 작성해보겠습니다. 

```c++
int _tmain(int argc, _TCHAR* argv[])
{
    FILE* fp;
    fp = fopen("test.bin", "rb");

    if(fp == NULL)
    {
        // 예외 처리
        return 1;
    }

    char buffer[512] = {0, };
    fread(buffer, 512, 1, fp);
    return 0;
}
```

대부분 위 코드처럼 작성하게 됩니다. 위 코드에서 누락된 예외처리 발견하신분? 예. 바로 fread 부분입니다. 

항목이 읽히지 않았을 때, fread는 0을 리턴하고, 0을 리턴했을 때에는 로그를 찍는다거나 등의 예외처리를 해주어야 파일을 잘못 읽었는지를 조기에 발견 할 수 있습니다. 

헌데 대부분의 수많은 예제는 첫 예제처럼 fread에 대한 검사는 이루어지지 않습니다. 바로 이 부분이 쉽게 놓치는 첫 감염입니다.  

감염을 조기에 발견하지 못하면, 숙주를 찾기 어려워지고 숙주를 찾지 못하면 전염병처럼 퍼지게 됩니다. 위 코드의 모범적 예외 처리 방식은 아래와 같습니다. 

```c++
    int _tmain(int argc, _TCHAR* argv[])
    {
        FILE* fp;
        fp = fopen("test.bin", "rb");

        if(fp == NULL)
        {
            // 예외 처리
            return 1;
        }

        char buffer[512] = {0, };
        int read_count = fread(buffer, 512, 1, fp);
        if(read_count == 0)
        {
            // 예외 처리
            return 2;
        }
        return 0;
    }
```

이렇게 되어야 buffer의 데이터가 유효하게 로직으로 전달되었는지 검사할 수 있습니다.  

혹자는, 컨텐츠 코드에서 방어적으로 데이터가 유효한지 검증해야 되는게 아니냐라고 하실 수도 있습니다.  

하지만, 그 당연하다 여겨지는 전치검사/후치보장은 막상 그렇게 당연하게 여겨지지 않는게 현실입니다. 

심지어 클래스 내부 멤버에 대해선 전치검사 대상으로 여겨지지 않는 경우가 태반입니다. 게다가 작성하는 API 함수 및 프로젝트에서 구현한 함수마다 사용법및 예외 처리 방식을 검토하는 일은 매우 번거롭고 실수가 있을 수 밖에 없죠. 

같은 프로젝트내에 리턴값 및 예외 처리 방식이 조금만 다르게 작성 (WIN32 API에서의 DWORD형의 결과값과 COM에서 HRESULT 값이 주로 그런 케이스. 어떤 매크로로 검사해야될지부터 혼동이 옴.) 된 걸로도 충분히 혼란스러운데, 여러개의 서드파티 솔루션까지 사용하는 규모가 큰 프로젝트의 경우 그 피로도가 장난이 아니죠. 

그 대안으로 대부분의 언어에서는 예외 처리 기능을 지원합니다. c++의 경우 try-catch(try-except는 SEH)가 그 역할을 하는데요, boost나 STL의 경우에는 내부적으로 오류를 감지하면 throw로 예외를 던지고 있습니다. 말로만 하면 감이 안오실테니 코드로 보여드리겠습니다. 

```c++
int _tmain(int argc, _TCHAR* argv[])
{
    try
    {
        FILE* fp;
        fp = fopen("test.bin", "rb");

        if(fp == NULL)
        {
            throw std::exception("file open failed.");
        }

        char buffer[512] = {0, };
        int read_count = fread(buffer, 512, 1, fp);
        if(read_count == 0)
        {
            throw std::exception("file data read failed.");
        }
    }
    catch(const std::exception& e)
    {
        std::cout << e.what() << std::endl;
        return 1;
    }
    return 0;
}
```

사용자 정의 exception 클래스를 이용할 수도 있지만, 여기선 쉬운 예제를 위해 std::exception 클래스를 통해 예외처리를 했습니다. 사실 이렇게 간단한 예제로는 예외처리가 뭐가 더 낫다는건지 감이 잘 오지 않습니다. 저도 그랬으니까요. 자~ 코드가 복잡해집니다. 다양한 기능이 붙고, 추상화니, 메시지 핸들러니 각종 쌈박한 로직을 구현해두었습니다. 만들다보니 기능이 붙고 붙네요. 외부 라이브러리도 붙었습니다. 정규식은 boost쓰고, 컨테이너는 stl 쓰고, json은 json spirit, xml은 tinyxml 을 붙였군요. 제가 이 글을 쓰고자 했던건 사실 tinyxml 때문이었습니다. tinyxml 물론 잘 만들어진 라이브러리입니다. 저도 아주 잘 쓰고 있고요. 헌데 이 tinyxml에서 문제가 하나 있었습니다. 

```c++
TiXmlElement* pNickname = pPlayerElem->FirstChildElement("nickname");
if (pNickname  == NULL)
    return false;

std::string nickname = pNickname >GetText();
```

문제가 된 부분이 보이시나요? 전 몇번이나 코드를 훓어보던 중에야 원인을 찾을 수 있었습니다. 

원인은 pNickname >GetText() 이 함수가 문제였습니다. 

위 코드는 nickname을 문자열형으로 읽어내는 코드입니다. 위 코드는 정상적으로 데이터가 포함되어있을 땐(<Nickname>엘키</Nickname>) 문제가 없습니다.   헌데 만약 이렇게 비어있는 값(<Nickname></Nickname>)일 때 읽는다면? 문제가 생겼습니다. 

비어있을 때 문제가 생긴다는 것도, 코드를 자세히 들여다 보고 나서야 확인한 것이지, 덤프를 본 직후에 바로 알아볼 순 없었어요. 

한 줄 한 줄 코드를 검토하며 함수 내부를 들여다 본 순간… 아차 싶었습니다.

```c++
    const char* TiXmlElement::GetText() const
    {
        const TiXmlNode* child = this->FirstChild();
        if ( child ) {
            const TiXmlText* childText = child->ToText();
            if ( childText ) {
                return childText->Value();
            }
        }
        return 0;
    }
```

child->ToText() 함수가는 내부에 포함된 데이터가 비어있을 때 NULL이 반환되고, 이어 함수 최하단에서 return 0; 코드를 타 NULL 포인터가 반환되었습니다. 

사실 이 함수가 반환타입이 const char* 인 만큼 return ""; 으로 빈 문자열을 반환할 줄 알았습니다.  하지만 빈문자열과 child가 없는 등의 Text화 실패에 대한 상황을 구분하기 위해 return 0; 으로 처리하고 있더군요. API를 꼼꼼하게 체크해보지 못한 개발자의 잘못도 있지만 구현상의 애매모호함도 엄연히 존재한다고 봅니다. 

1. 오류 상황과 데이터가 없음을 모두 return 0; 즉 NULL 포인터로 반환하고 있는 문제
2. const char*형은 문자열형만 반환될뿐 사실은 pointer의 개념보다는 문자열 첫 주소가 반환된다는 의미로도 인식되어, NULL 포인터가 반환될 거라 인식하기 어려운 문제를 가지고 있는것이죠.

결과적으로 GetText() 함수의 구현 코드를 보니 NULL 포인터가 반환되는지 검사해주어야 했습니다.

위 코드를 예외처리해봅시다.

```c++
TiXmlElement* pNickname = pPlayerElem->FirstChildElement("nickname");
if (pNickname  == NULL)
    return false;

if(pNickname->GetText() == NULL)
    return false;

std::string nickname = pNickname >GetText();

TiXmlElement* pUserID = pPlayerElem->FirstChildElement("user_id");
if (pUserID  == NULL)
    return false;

if(pUserID >GetText() == NULL)
    return false;

std::string user_id= pUserID>GetText();
```

이렇게 해주어야합니다. GetText를 사용하는 코드가 많으면 많을수록 if문은 늘어납니다. 물론 위의 if문을 

```c++
if (pUserID  == NULL || pUserID >GetText() == NULL)
    return false;
```

or 연산으로 수정할수야 있겠지만, 그렇다손쳐도 GetText() 하는 함수마다 체크해주어야 함은 변함이 없습니다. 

```c++
const char* TiXmlElement::GetText() const
{
    const TiXmlNode* child = this->FirstChild();
    if ( child == NULL) 
    {
        throw std::exception("TiXmlElement::GetText() FirstChild is NULL");
    }

    const TiXmlText* childText = child->ToText();
    if ( childText == NULL) 
    {
        throw std::exception("TiXmlElement::GetText() ToText Failed.");
    }
    return childText->Value();
}
```

이렇게 코드를 작성했다면 어떨까요? 

```c++
try
{
    TiXmlElement* pNickname = pPlayerElem->FirstChildElement("nickname");
    if (pNickname  == NULL)
        continue;

    std::string nickname = pNickname >GetText();

    TiXmlElement* pUserID = pPlayerElem->FirstChildElement("user_id");
    if (pUserID  == NULL)
        return false;

    std::string user_id= pUserID>GetText();
}
catch(const std::exception& e)
{
    std::cout << e.what() << std::endl;
    // 일괄적인 예외 핸들링
    return false;
}
```

만약 try안에서 호출된 다른 함수에서 throw가 존재한다해도 코드의 흐름을 멈추고, 원인을 알아낼 수 있게 됩니다.  

이렇게 되면 예외처리를 일괄적으로 할 수 있고, 코드의 흐름을 중단하는 역할도 맡길 수가 있습니다. 

 보통 외부 라이브러리 함수에서 크리티컬한 상황이 발생했을 경우라거나, 유틸리티 함수등에서의 실패를 리턴값으로 반환하곤 하는데, 이보다 더 강하게 코드의 진행을 중지 시키고 싶을 때 (2차 감염을 막고, 현재 상황의 위험성을 알리기 위해서 주로 이렇게 하죠) 라고 볼 수 있습니다.  

리턴값은 그 함수를 사용하는 코드마다 체크 로직과 핸들링 로직을 구현해주어야 합니다. 

이에 비해 예외 처리는, 적절한 위치에 try-catch로 묶어주기만해도 throw로 던져진 예외를 핸들링 할수가 있게 됩니다.  

물론 모든 코드가 그렇게 예외처리를 하는 것은, 코드의 흐름을 원치 않는 곳에서 중단 시키기 때문에, 상황에 따라 국소적인 코드마다 try-catch 핸들링을 해주어야 하는 부작용도 있긴하지만, 순작용이 훨씬 많은 예외 처리 방식입니다. 
 심지어 이는 C++에 국한된 것이 아닌, 대다수의 언어에서 지원되는 예외처리 방식이므로 한번 몸에 익혀두시면 두고 두고 활용하시기에 좋습니다. 

C++도 표준 라이브러리들은 대부분 throw로 예외처리를 하기 때문에 try-catch로 예외처리를 할수있고, ruby등의 기타 스크립트 언어들도 내부 함수 예외를 try-catch로 핸들링 할 수 있습니다.  

try-catch 적용시 많이들 어렵게 생각하시는 점은, 어디서 부터 어디까지 try-catch로 감싸도 되는가에 대한 고민이라고 생각하는데요, 이는 boost나 stl등 내부적으로 throw를 사용하고 있는 함수들 부터 적용해보시면 어렵지 않게 적응하실 수 있습니다. 

코드의 흐름을 제어하고, 일반화된 오류 처리를 도와주는 예외처리. 적극적으로 써보시면 어떨까요?
