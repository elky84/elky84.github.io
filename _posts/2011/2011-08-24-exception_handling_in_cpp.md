---
layout: post
title: 윈도우 환경에서의 C++ 프로그램 예외처리
date: 2011-08-24 11:14:54
categories: C++ 예외처리 디버깅
comments: true
---

윈도우 예외 처리에 대한 정리 개요 윈도우에서 사용가능한 예외 처리로는 C++ 예외 처리와, SEH (Structured Exception Handling) 이 있습니다. 

일반적으로 SEH(Structured Exception Handling)이라고 말하면 Windows 자체적으로 지원하는 구조적 예외 처리를 의미합니다. (관련 키워드 : __try, __except, __finally, __leave) 

그리고 C++ Exception Handling (이하 C++ EH) 라 하면 C++ 에서 정의하고 있는 구조적 예외 처리를 의미합니다. (관련 키워드 : try, catch, throw) 두 예외 처리 방식에 대해 간단히 설명드리겠습니다. 

    __try
    {
        int a = 500;
        int b = a / 0; // 0으로 나누기
    }
    __except(GetExceptionCode() == EXCEPTION_INT_DIVIDE_BY_ZERO ?
                EXCEPTION_EXECUTE_HANDLER : EXCEPTION_CONTINUE_SEARCH)
    {
        // 예외 처리
    }

이 것이 기본적인 SEH 사용법입니다. 

__finally 키워드의 경우 예외가 발생하던 발생하지 않던 수행해야 되는 구문에서 쓰입니다. 

    DWORD FilterFunction()
    {
       printf("1 ");                     // printed first
       return EXCEPTION_EXECUTE_HANDLER;
    }
     
    void main(VOID)
    {
       __try
       {
           __try
           {
               RaiseException(
                   1,                    // exception code
                   0,                    // continuable exception
                   0, NULL);             // no arguments
           }
           __finally
           {
               printf("2 ");             // this is printed second
           }
       }
       __except ( FilterFunction() )
       {
           printf("3\n");                // this is printed last
       }
    }

위 코드 수행 후 1 2 3이 출력됩니다. 

즉, 예외 발생했음에도 __finaly안의 구문은 수행됐음을 의미하죠. 

    DWORD FilterFunction()
    {
        printf("1 ");                     // printed first
        return EXCEPTION_EXECUTE_HANDLER;
    }
     
    void main(void)
    {
        __try
        {
               __try
               {
                    // none         
               }
               __finally
               {
                       printf("2 ");             // this is printed second
               }
        }
        __except ( FilterFunction() )
        {
               printf("3\n");                // this is printed last
        }
    }

위 코드 수행은 2만 출력됩니다. 

예외 발생 유무에 상관 없이 __finaly 안의 구문은 수행되는 것이죠. 
__leave 구문은 __try 구문안에서 빠져나가고자 할 때 사용됩니다. 

    DWORD FilterFunction()
    {
        printf("1 ");                     // printed first
        return EXCEPTION_EXECUTE_HANDLER;
    }
     
    void main()
    {
        __try
        {
               __try
               {
                    printf("try");
                __leave;
                RaiseException(
                            1,                    // exception code
                            0,                    // continuable exception
                            0, NULL);             // no arguments
               }
               __finally
               {
                       printf("2 ");             // this is printed second
               }
        }
        __except ( FilterFunction() )
        {
               printf("3\n");                // this is printed last
        }
    }

위 코드 수행시 try 2 가 수행됩니다. __leave가 수행됐음에도 __finaly가 안에 포함된 코드가 수행됐음을 알 수 있죠. 이상이 SEH의 기본적인 사용법이었습니다. 


C++ EH 사용 예제 

    try
    {
        throw "Memory allocation failure!";
    }
    catch( char * str )
    {
        std::cout << "Exception raised: " << str << '\n';
    }

위 코드 수행시 Exception raised : Memory allocation failure! 문장이 출력됩니다. 

즉, try {} 는 수행 구문, catch(캐치할 예외 타입) {}, throw 예외 로 처리되는 것입니다. 여기서 주의할 점은 throw; 는 예외의 전파로써 사용이 되기도 한다는 점 입니다. 

여기서 모호한 점은 throw; 가 try안에서 사용될 때와, catch에서 사용 될 때와 차이가 있다는 점입니다. 

    void main(VOID)
    {
       try
       {
           try
           {
               throw "Memory allocation failure!";
           }
           catch( char * str )
           {
               std::cout << "Exception raised: " << str << '\n';
               throw;
           }
       }
       catch(...)
       {
           std::cout << "catched" << '\n';
       }
    }

위 코드의 경우가 catch에서 예외의 재전파에서 사용되는 예입니다. 

위 구문 수행시 Exception raised : Memory allocation failure! catched 가 출력됩니다. 만약 예외만 발생시키려 throw; 를 try안에서 사용했을경우를 볼까요? 

    try
    {
        try
        {
            throw;
        }
        catch( ... )
        {
            std::cout << "Exception raised: \n";
        }
    }
    catch(...)
    {
        std::cout << "catched" << '\n';
    }

위 코드를 수행할 경우 예외가 발생하게 됩니다.


C++ 표준 15.1.8에 따르면
"If no exception is presently being handled, executing a throw-expression with no operand calls std::terminate()."
즉, 현재 예외가 없는데 throw; 하면 프로그램이 종료됩니다. 예외는 함수 경계를 넘어서 전파될 수 있으므로 throw; 가 문법적으로 catch 안에 있을 필요는 없습니다.

위 내용에 대해서는 아래 링크를 따라가 보시면, 논의가 이루어졌습니다.

KLDP의 try 안에서의 throw에 대한 논의 : http://kldp.org/node/106380

# SEH to C++ EH

SEH 를 C++ Exception으로 자동으로 변환하도록 만들었을 때의 장점은 0xC0000005 같은 잘못된 메모리 참조 예외같은 하드웨어 예외까지도 C++ EH를 사용해서 한곳에서 감지할 수 있다는 점이 될 수 있겠죠.

다행이도 Windows Exception이 발생했을 때 콜백 받을 수 있는 함수가 존재하므로, 아주 간단한 구현이 가능합니다.

    _set_se_translator( TranslateSEHtoCE );

위와 같이 해주면, Windows Exception이 발생할 때마다 TranslateSEHtoCE 이라는 이름의 함수가 호출됩니다.

TranslateSEHtoCE안에서는 C++ Exception을 발생시키면 되겠죠.
  

    // a C++ exception class that contains the SEH information
    class CSEHException
    {
    public:
           CSEHException( UINT code, PEXCEPTION_POINTERS pep)
           {
                      m_exceptionCode        = code;
                      m_exceptionRecord    = *pep->ExceptionRecord;
     
                      m_context            = *pep->ContextRecord;
     
                  _ASSERTE(m_exceptionCode == m_exceptionRecord.ExceptionCode);
           }
     
           operator unsigned int() { return m_exceptionCode; }
     
           // same as exceptionRecord.ExceptionCode
           UINT m_exceptionCode;
     
           // exception code, crash address, etc.
           EXCEPTION_RECORD m_exceptionRecord;
     
           // CPU registers and flags
           CONTEXT m_context;
    };
     
    // the SEH to C++ exception translator
    void _cdecl TranslateSEHtoCE( UINT code, PEXCEPTION_POINTERS pep)
    {
          throw CSEHException(code, pep);
    }
     
    int main(int argc, char* argv[])
    {
           // install the translator
           _set_se_translator( TranslateSEHtoCE);
     
     
           try
           {
                      char* p = NULL;
     
                  *p = 'A';
           }
           catch( CSEHException& e)
           {
     
                  if( EXCEPTION_ACCESS_VIOLATION == e)
                      {
     
                          _RPT0( _CRT_WARN, "Access Violationn");
                      }
           }
           return 0;
    }

# 참고 자료 
[Serious-Code SEH](http://serious-code.net/moin.cgi/SEH)
[Microsoft Exception](http://www.microsoft.com/msj/0197/exception/exception.aspx)
[MSDN SEH](http://msdn.microsoft.com/en-us/library/ms680657%28v=vs.85%29.aspx)
[MSDN C++ Exception Handling](http://msdn.microsoft.com/en-us/library/4t3saedz%28v=vs.71%29.aspx)
[MSDN의 Managed Exception](http://msdn.microsoft.com/ko-kr/library/ms384202%28v=vs.71%29.aspx)
[/EH (예외처리 모델)](http://msdn.microsoft.com/ko-kr/library/1deeycx5%28v=vs.80%29.aspx)