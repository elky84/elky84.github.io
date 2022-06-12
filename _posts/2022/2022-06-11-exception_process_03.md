---
layout: post
title: 예외처리 03 - try-catch 그리고 throw
date: 2022-06-11 00:00:00
categories: [Exception, Exception Process, Log]
tags: [Exception, Exception Process, Log]
comments: true
---

예외 처리를 잘 하기 위해선, try-catch, throw를 잘 하는 것이 중요하다.

혹자는 try-catch 구문이 코드가 지저분해 보이거나, 복잡해 보인다고 기피하는 경향을 보이기도하고, C++을 오래 써온 개발자는 throw가 성능 저하를 가져다 주는 무거운 동작이라고 기피하기도 한다.

하지만, 지금 시대에 와서 예외는 수 많은 표준 라이브러리들이 적극 사용하는 만큼, 이에 대해서 적극적인 사용을 해야 한다.

우리가 왜 그래야 하는걸까?

## try-catch의 중요성

1. 내가 만들지 않은 기능에서 예외가 발생 할 수 있다.
    - 우리는 수 많은 기능을 가져다 쓰고 있다.
    - 표준 API 라고 할 지라도, 사용법을 오인해서, 혹은 내부적인 제약에 빠져서 믿고 쓰는 File 관련 API 같은 함수에서도 예외는 충분히 발생 할 수 있다.
2. 내가 만든 기능에서 의도치 않은 예외가 있을 수 있다.
    - devide by zero
    - null pointer 접근
    - 기타 Exception 종류
        - [Exceptions in Java - GeeksforGeek](https://www.geeksforgeeks.org/exceptions-in-java/)
    - Microsoft Using Standard Exception Types
        - [Using Standard Exception Types - Framework Design Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/using-standard-exception-types)
    - Oracle Exception 처리 가이드
        - [Lesson: Exceptions (The Java™ Tutorials > Essential Java Classes) (oracle.com)](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)


스프링을 포함 한 각종 웹 프레임워크만 떠올려봐도 기본 Exception Handler를 설치해서 로직에서 발생하는 수 많은 오류를 캐치해준다.

이런 메커니즘에는 예외라는건 내가 코드를 매우 안전하게 짰다고 자부할 정도로 노력했음에도 발생할 수 있음을 가정하고 구현 된 것이다.

보통 프레임워크를 쓰게 되면, 기본 예외 처리기를 사용하던, Global Exception Handler를 추가로 설정해서 예외에 대한 응답을 일치 시키는 노력을 하곤한다.

하지만 프레임 워크를 이용하지 않고 작성된 어플리케이션에서는 예외처리를 꺼리는 경우를 많에 보게 된다.

try-catch로 이어지는 구문이 코드가 우아해보이기 보이지 않는 것은 인정하지만, 그럼에도 매우 매우 중요하다.

그래서 예외 처리를 코드 실행 최상 위에 숨기더라도 반드시 해야 하는 요소라고 볼 수 있다.

또한 멀티 스레드 환경이라면, 실행되는 스레드 실행 지점마다 try-catch로 묶어 주는 것이 좋다. 프레임 워크 내부 코드가 실행 지점이라면, 내 코드와 연결되는 결합 지점이라도 try-catch로 묶어주어야 처리되지 않은 예외로 인한 크래시를 막을 수 있다.

## throw가 필요한 상황

여기까지가 try-catch의 필요성과 설치 지점에 대한 이야기였고, 다음 이야기는 throw다.

어떠한 상황에서 직접 throw를 하는게 좋을까?

1. 함수 반환 값만으로 실패를 전달하기엔 무리가 있기에, 기존 interface를 유지한채 예외를 던질 때
    - 많은 함수는 구현시에 값을 반환하게 되어있으며, 이 값이 null인 것만으로는 성공 실패를 알리기 어렵다.
    - tuple, pair 등의 값으로 반환하는 것도 방법이다. 하지만 이 경우에도 depth가 깊은 경우, 모든 함수에서 인터페이스를 결과 코드를 반환하게끔 코드 작성하는 것은 쉬운게 아니다.
    
    ```csharp
    public enum ResultCode
    {
      Success,
      Fail,
      // ... 기타 결과 코드
    }
    
    Tuple<Obj, ResultCode> DoRealSomething()
    {
    	if(실패)
    	{
    		return new Tuple<Obj, ResultCode>(null, ResultCode.Fail);
    	}
    
    	return new Tuple<Obj, ResultCode>(obj, ResultCode.Success);
    }
    
    Tuple<Obj, ResultCode> DoSomething()
    {
       return DoRealSomething();
    }
    ```
    
    - 위와 같은 인터페이스를 유지하는 비용에 가장 큰 문제는, 해당 반환 값을 거치는 모든 함수의 인터페이스를 변경해야 함에 있다.
        - 이는 반환 값을 통한 예외 처리를 위해 불필요한 규칙을 추가해야 함을 의미한다.
    - 그래서 다음과 같이 오류가 발생시 직접 throw를 호출해 예외를 알리는 것이 더 좋은 방법이다.
        
        ```csharp
        public enum ResultCode
        {
          Success,
          Fail,
          // ... 기타 결과 코드
        }
        
        Obj DoRealSomething()
        {
        	if(실패)
        	{
        		return throw new CustomException(ResultCode.Fail);
        	}
        
        	return obj
        }
        
        Obj DoSomething()
        {
           return DoRealSomething();
        }
        ```
        
2. 예외 클래스별로 catch가 되기 때문에, 예외 종류별로 정의된 클래스마다 처리 규칙, 의미를 부여할 수 있다.
    - LogicException (결과 코드를 처리하기 위한 익셉션)
        
        ```csharp
        public class LogicException : Exception
        {
          public Result Result { get; set; }
        
          public LogicException(Result result)
          {
            Result = result;
          }
        
          // 기타 생성자
        }
        ```
        
    - 해당 사용자에 대한 요청을 거부해야 될만큼의 문제를 일으킨 상황에서의 예외
        
        ```csharp
        public class KickException : Exception
        {
          public Reason Reason { get; set; }
        
          public KickException(Reason reason)
          {
            Reason = reason;
          }
        
          // 기타 생성자
        }
        ```
        
    - 특정 패턴이 서버 성능에 악영향을 줄 수 있어, 거절해야 될 요청에 대한 익셉션
        
        ```csharp
        public class DeniedException : Exception
        {
          public Reason Reason { get; set; }
        
          public DeniedException(Reason reason)
          {
            Reason = reason;
          }
        
          // 기타 생성자
        }
        ```
3. 예외에 대한 부가정보는 최대한 많이 전달하는게 좋다
   
   ```csharp
   public class DeniedException : Exception
   {
     public Reason Reason { get; set; }
   
     public Dictionary<string, string> RelationVaraibles { get; set; }
   
     public DeniedException(Reason reason, Dictionary<string, string> relationVaraible )
     {
       Reason = reason;
   		RelationVaraibles = relationVaraible;
     }
   
     // 기타 생성자
   }
   ```
        

이렇게 정보를 확장하면 다른 개발자에게 의미 전달, 디버깅을 위한 정보 취합, 기존 코드의 반환값 규칙을 보장하면서 예외를 처리할 수 있다.

이렇게 try-catch의 필요성과, throw를 잘 이용하기 위한 방법에 대해서 알아보았다.