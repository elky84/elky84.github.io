---
layout: post
title: C프로그래머가 알아야 할 것들 - 08 프로세스와 스레드
date: 2008-01-10 00:00:08
categories: [C프로그래머가 알아야 할 것들]
tags: [C프로그래머가 알아야 할 것들]
comments: true
---

### 프로세스와 스레드
스레드를 이해하려면 프로세스에 대한 이해가 선행되어야 합니다. 프로세스란 프로그램이 실행되는 단위를 말합니다. 지금 제가 이 문서를 작성하고 있는 OpenOffice도 프로세스고, 음악을 듣고 있는 aimp2도 프로세스, 메신져인 pidgin 모두 프로세스입니다. 일반적으로 프로그램의 실행 단위가 프로세스라고 보시면 됩니다. (한 프로그램 내에 여러 프로세스를 묶어 하나처럼 보이게 하는 경우도 있지만, 이런 경우는 예외로 생각하겠습니다.)
 
저는 지금 메신져를 켜놓고, 음악을 들으며 문서 작성을 하고 있는데요, 이렇게 세가지 작업을 한꺼번에 할 수 있는 것은, 윈도우즈가 멀티 프로세스를 지원하기 때문입니다.

반면 윈도우즈 자체는 싱글 프로세스입니다. 메모리에 대한 소유권, CPU 제어권을 혼자 독점합니다. 그렇게 독점한 자원을 바탕으로, 프로세스들을 실행/관리 해주는 것이죠.

그렇다면 스레드는 무엇일까요? 프로세스 내의 실행 단위를 말합니다. C언어 계열의 프로그램은 기본적으로 main() 함수에서 시작합니다. 그리고, main()함수 에서부터 순차적으로 실행되죠. main()부터 시작되는 스레드를 메인 스레드라 부릅니다.

메인 스레드 외에 다른 스레드와 교차되어 실행 되는 것이 멀티 스레드 입니다. 여기서 키 포인트는 '교차' 입니다. 스레드가 교차되어 실행 되기에 파생되는 문제점 들에 대해서 자세히 얘기해보죠.

### 멀티 스레드에서 교차 실행에 따른 문제점
교차된다는 게 무엇일까요? 직렬이 아니라 병렬이라는 뜻이겠죠? 스레드를 여러개 만든 다는 것은 코드의 실행 흐름을 여러개 만든다는 의미가 됩니다. main()에서 시작되서, main()내의 코드 흐름만을 따르는게 아니라, 스레드를 만드는 수 만큼 코드 흐름이 늘어나는 것이 되죠.

흐름을 여러개 만들어놓고, 한 흐름이 끝날 때까지 기다려야 한다면 직렬. 즉 단일 스레드와 전혀 다를바 없어집니다.

그래서, 각 스레드가 실행 되던중에, 다른 스레드로 실행 흐름이 옮겨가게 되고, 이 것을 교차라고 표현한 것입니다.

    #include <Windows.h>

    int g_nTest = 0;

    DWORD WINAPI IdleThread(void *lpVoid)
    {
        while(g_nTest < 30)
        {
            printf("%s %d\n", __FUNCTION__, ++g_nTest);
        }
        return 0;
    }

    int _tmain(int argc, _TCHAR* argv[])
    {
        HANDLE hThread = CreateThread(NULL, 0, ::IdleThread, NULL, 0, NULL);

        while(g_nTest < 30)
        {
            printf("%s %d\n", __FUNCTION__, ++g_nTest);
        }

        if(hThread != INVALID_HANDLE_VALUE)
        {
            WaitForSingleObject(hThread, INFINITE);
            CloseHandle(hThread);
        }
        return 0;
    }


위 코드는 스레드를 하나 추가로 생성하고, 전역 변수를 0에서 시작해서 100이 될 때까지 실행 되도록 한 코드입니다.

실행 결과는 어떨까요?

![image_01](/img/2008/multithread_01.png)
![image_02](/img/2008/multithread_02.png)


어떤 스레드에서 g_nTest값을 썼던간에, g_nTest에 해당 하는 값은 1~30까지 순차적으로 찍혀야 된다고 생각하시죠? 하지만 실제 수행 결과는 차이가 있습니다.

wmain() (= main(). 이하 main())의 while문이 실행 되던중 g_nTest 변수가 13이었을 때, IdleThread의 printf 구문이 실행되었고, g_nTest변수를 ++g_nTest 시킨 상태에서 printf()에 값을 전달했으나 아직 printf()가 실행되지 않은 상태에서 코드 흐름이 main()으로 다시 넘어가 30까지 실행 되었습니다.

그리고 다시 코드 흐름이 IdleThread()로 넘어오게 오게 되어, printf()함수에 넘겼던 %d에 해당하는 값인 14가 출력되고 프로그램이 종료 되었죠.

여기서 핵심은 g_nTest 변수가 13이었을 때 IdleThread의 ++g_nTest 구문이 실행되었다는 점입니다. main스레드의 while 루프를 돌던 중에, 코드 수행이 IdleThread로 옮겨 간 것이죠.
정확히 하자면 스레드는 병행 수행이 아니라, 스레드의 수만큼 실행 흐름을 만들고, 스레드의 동작 도중 다른 스레드로 실행 흐름이 바뀐다는 것입니다.

그런데 이 스레드의 교차 수행으로 인해 장점이자 단점이 생깁니다. 뭘까요?

(3) 교차와 메모리 공유에서 오는 문제들

멀티 스레드라고 해도, 메모리 할당은 프로세스 단위로 이루어지기 때문에 같은 메모리를 공유하게 되어있습니다.

게다가 교차되서 실행 되기에 같은 메모리에 덮어 썼을 때 실행 결과가 예상과 달라질 수 있게 됩니다.

예제 코드를 보시죠.

    #include <Windows.h>

    int *g_pTest = new int(0);

    DWORD WINAPI IdleThread(void *lpVoid)
    {
        while(g_pTest && *g_pTest < 20)
        {
            printf("%s %p %d\n", __FUNCTION__, g_pTest, *g_pTest);
            (*g_pTest)++;
        }

        delete g_pTest;

        g_pTest = NULL;
        printf("%s %p delete g_pTest\n", __FUNCTION__, g_pTest);
        return 0;
    }

    int _tmain(int argc, _TCHAR* argv[])
    {
        HANDLE hThread = CreateThread(NULL, 0, ::IdleThread, NULL, 0, NULL);

        while(g_pTest && *g_pTest < 20)
        {
            printf("%s %p %d\n", __FUNCTION__, g_pTest, *g_pTest);
            (*g_pTest)++;
        }

        if(hThread != INVALID_HANDLE_VALUE)
        {
            WaitForSingleObject(hThread, INFINITE);
            CloseHandle(hThread);
        }

        getchar();
        return 0;

    }


이 코드는 메모리 공유로 인한 문제를 보여줍니다. g_pTest를 IdleThread에서, 20까지 증가 시킨 후, g_pTest를 초기화 합니다. main()함수에는 g_pTest가 유효할때만 로그를 찍고, 값을 1 증가 시켜주죠.

하지만, IdleThread가 먼저 종료되고 난 후, 메인 스레드로 복귀 했을때, 수행중이었던 첫번째 printf은 무난하게 넘어가지만 (스택에 이미 값을 넘긴 이후기에), 두번째 printf에서, (*g_pTest)에 ++를 하려다 NULL포인터 접근 오류가 발생했습니다.

![image_03](/img/2008/multithread_03.png)
![image_04](/img/2008/multithread_04.png)

이상하죠? 메모리 공유나 교차 문제가 아니라면, while(g_pTest)에서 널포인터 검사를 했기에 유효한 포인터여야 할텐데 라고요.

하지만, g_pTest가 전역 데이터이기에 쓰레드간에 공유됩니다. 그래서 main()쓰레드가 첫번째 printf함수에 값을 전달해놓은 상태에서 멈췄고, IdleThread가 수행되며 g_pTest가 NULL이 되고 난 후에야 main()로 코드 흐름이 돌아왔고, 그래서 NULL포인터 접근 오류가 발생 한 것입니다. (이 예제 코드 수행 시 항상 NULL포인터 접근 오류가 발생하진 않다는 점에 유의하십시오.)

현재 두가지 경우 모두다 전역 변수/포인터로 설명했지만, 함수에 포인터로 전달된 값들도 같은 문제를 안고 있습니다.

그러면 이 문제를 해결하기 위해 어떻게 해야 할까요? 이 문제의 해결책은 바로 스레드 동기화입니다.

### 스레드 동기화
우선 우리의 문제부터 다시 정리해보면 공유된 메모리를 사용하는데, 다른 스레드가 언제 끼어들지 몰라 어떤 값이 언제 변할지 모른다는 점이다.

이 문제를 해결하기 위해 일반적으로 어떤 값에 대한 사용이 끝나기 전까지는, 그 값을 사용하려는 다른 스레드가 멈춰있게 함으로써 그 값에 대한 신빙성을 유지해주는 방법을 쓴다.

이 방식으로 데이터 신빙성을 유지해주는 방법을 크리티컬 섹션 동기화 방식 (이하 CRITICAL_SECTION또는 크리티컬 섹션)라고 합니다.
다음 코드를 보시죠.

    #include <Windows.h>

    CRITICAL_SECTION cs;

    int *g_pTest = new int(0);

    DWORD WINAPI IdleThread(void *lpVoid)
    {
        printf("%s Start\n", __FUNCTION__, g_pTest);

        EnterCriticalSection(&cs);

        while(g_pTest && *g_pTest < 20)
        {
            printf("%s %p %d\n", __FUNCTION__, g_pTest, *g_pTest);
            (*g_pTest)++;
        }

        delete g_pTest;

        g_pTest = NULL;

        printf("%s %p delete g_pTest\n", __FUNCTION__, g_pTest);

        LeaveCriticalSection(&cs);
        return 0;
    }


    int _tmain(int argc, _TCHAR* argv[])
    {
        InitializeCriticalSection(&cs);

        HANDLE hThread = CreateThread(NULL, 0, ::IdleThread, NULL, 0, NULL);

        EnterCriticalSection(&cs);

        while(g_pTest && *g_pTest < 20)
        {
            printf("%s %p %d\n", __FUNCTION__, g_pTest, *g_pTest);
            (*g_pTest)++;
        }

        LeaveCriticalSection(&cs);


        if(hThread != INVALID_HANDLE_VALUE)
        {
            WaitForSingleObject(hThread, INFINITE);
            CloseHandle(hThread);
        }

        DeleteCriticalSection(&cs);
        getchar();
        return 0;
    }


이 코드는 크리티컬 섹션으로 데이터 사용중임을 다른 스레드에 알린다는 점을 제외하고는 위 예제와 동일합니다.

그리고 이렇게 수정함으로써 메모리 접근 오류도 발생하지 않게 되죠.

코드 수행 결과부터 보시죠.

![image_05](/img/2008/multithread_05.png)

EnterCriticalSection() 은 같은 CRITICAL_SECTION 객체를 사용하지 않는 상태에서만 수행 됩니다. 어딘가에서 사용할려는 CRITICAL_SECTION 객체가 사용중이라면, 그 사용이 끝날때까지 대기 합니다.

그래서 IdleThread가 수행되려 했음에도 불구하고, main스레드의 while문이 종료 될때까지 대기한 후, 종료 되고 나서야 IdleThread가 실행 되었죠.

여기서 중요한 것은 g_pTest에 변경을 시도하는 코드들이 모두 EnterCriticalSection()과,  LeaveCriticalSection()로 감싸져 있다는 점입니다.

만약 지금의 수행결과와 다르게 IdleThread가 main스레드보다 먼저 실행되어, IdleThread에서 g_pTest를 NULL로 만든 후에 main스레드로 돌아온다고 해도, while문 실행 도중에 main스레드로 돌아오는 것이 아니라, while문의 시작부분 g_pTest의 포인터 유효성 검사 코드 부터 수행 되어, NULL포인터 접근 하는 일은 없게 됩니다.

정리하자면, CRITICAL_SECTION 방식의 동기화는 “내가 데이터 사용 끝낼 때 까지, 다른 녀석들은 쓰지마!”로 요약할 수 있는 것이죠.

이 방식 이외에도 이벤트, 세마포어, 뮤텍스 등을 이용한 동기화 방법들이 존재하지만, 커널 모드/ 유저 모드에 대한 설명도 필요하고, 이 강좌에서 집중하려는 내용에서 벗어나기에 해당 내용은 차 후에 다루도록 하겠습니다.

그렇다면...이렇게 여러 문제가 있어, 동기화도 해주어야 하는 번거로운 녀석인 멀티 스레드가 필요할 때와, 필요하지 않을 때는 과연 언제 일까요?

### 멀티 스레드를 사용해야 할 때
몇년전만해도 멀티 스레드 프로그램은 지금보다 훨씬 적은 수 였습니다.

멀티 스레드가 다루기 어렵기 때문에 포기한 경우도 적지 않았는데요, 위에서 말한  문제를 비롯해서 데드락, 스레드 동기화를 위한 대기로 인한 성능 저하, 스레드 동기화 실패 등의 문제등이 발생할 수 있죠.

많은 문제 발생의 소지가 있음에도 불구하고 현재의 많은 프로그램들은 멀티 스레드를 선택하고 사용하고 있는데 그 이유는 멀티 스레드가 가진 장점들 때문이죠.

그 장점중 하나로는 블럭 함수들은 스레드 단위로 블럭에 걸린다는 것입니다.

다음 코드를 보시죠.

    #include <Windows.h>

    #pragma comment(lib, "winmm.lib")

    int _tmain(int argc, _TCHAR* argv[])
    {
        printf("Are you want to quit? You type 'Q' or 'q'\nIf you want Replay? You type other key.\n");

        while(1)
        {
            sndPlaySound(_T("Play.wav"), SND_SYNC);
            char key = getchar();
            if(key == 'q' || key == 'Q')
                break;
        }

        return 0;
    }

별도로 스레드를 만들어주지 않았기에 싱글 스레드 코드입니다. 이 코드의 경우 sndPlaySound()의 음악 재생이 끝날 때 까지 리턴되지 않습니다. 음악 재생이 끝난 이후라도 getchar()가 블럭 함수이기 때문에, 키 입력이 들어오기전에는 멈춰있게 되죠. 둘다 블럭되는 동작이기에 서로 동시에 이루어질 수 없죠.

그럼 멀티 스레드는 어떻게 될까요?

    #include <Windows.h>

    #pragma comment(lib, "winmm.lib")

    DWORD WINAPI IdleThread(void *lpVoid)
    {
        while(1)
        {
            sndPlaySound(_T("Play.wav"), SND_SYNC);
        }
        return 0;
    }

    int _tmain(int argc, _TCHAR* argv[])
    {
        HANDLE hThread = CreateThread(NULL, 0, ::IdleThread, NULL, 0, NULL);

        printf("Are you want to quit? You type 'Q' or 'q'. If you type other key, Sound Plays is Loop.\n");

        while(1)
        {
            char key = getchar();
            if(key == 'q' || key == 'Q')
                break;

        }

        if(hThread != INVALID_HANDLE_VALUE)
        {
            WaitForSingleObject(hThread, 1000);
            CloseHandle(hThread);
        }

        return 0;
    }

IdleThread는 계속 음악 재생 역할을 하고 있고, 메인 스레드는 루프가 돌면서 q또는 Q가 입력 되길 기다리고 있습니다. 멀티 스레드에서는 음악 재생중임에도 불구하고 키 입력이 가능해지는 것을 아실 수 있습니다.

이처럼 멀티 스레드를 쓴다는 것 자체가 한 스레드가 블럭 상태에 놓여있어도 다른 스레드는 영향을 받지 않는다는 큰 장점을 가지고 있습니다.

현재는 음악 재생이었지만, 큰 용량의 파일을 읽어서 분석한다고 해봅시다. 그 파일을 완전히 읽고 분석을 끝내기 전까지 그 데이터는 의미를 갖지 못합니다.

그런데 굳이 프로그램 전체가 멈춰가며 기다려야만 할까요? 아니죠?

그럴때 스레드를 생성한 후 생성한 스레드는 파일 로딩에만 집중하도록 한 후, 다른 스레드는 기존에 하던일을 처리하도록 만들면 로딩이 끝날 때까지 다른일을 할 수 있게 됩니다.

그런데...이렇게 좋은 스레드가 뭐가 문제라는 걸까요??

### 멀티 스레드를 쓰는 것이 독이 될 때

스레드가 실행 흐름이 여러개로 나뉘어진다고 했죠? 그 여러개의 실행 흐름을 갖기 위해서, 또 동시에 실행되기 위해선 스택과 스레드 제어 블록(어떤 스레드가 먼저 실행 되어야 하는지 등의 다른 스레드와의 관계 등에서 쓰이는 데이터)을 스레드마다 가져야 합니다.

스레드가 하나 추가 될 때 마다 스택과, 스레드 제어 블록도 같이 생성 되는데, 이로 인한 부하가 사실 만만치않습니다. 또한, 스레드 간 스위칭 (다른 스레드로 실행 흐름이 옮겨지는 것을 스위칭이라 합니다)에도 적지 않은 비용이 듭니다.

게다가 동기화를 잘못 했을 경우, 다른 스레드를 기다리는 시간이 길어져 속도가 저하되는 문제도 발생할 수 있죠.

크리티컬 섹션이 데이터 사용 끝낼 때 까지 다른 사람은 건드리지말라는 의미라 말씀드렸죠? 크리티컬 섹션을 쓴다는 것은, 같은 크리티컬 섹션 객체를 사용하는 다른 스레드가 대기 상태에 빠지게 합니다. 즉 해당 스레드가 그 시간만큼 기다려야 한다는 것이죠.

    #include <Windows.h>

    #pragma comment(lib, "winmm.lib")

    CRITICAL_SECTION cs;

    DWORD WINAPI IdleThread(void *lpVoid)
    {
        while(1)
        {
            EnterCriticalSection(&cs);
            sndPlaySound(_T("Play.wav"), SND_SYNC);
            LeaveCriticalSection(&cs);
        }

        return 0;
    }

    

    int _tmain(int argc, _TCHAR* argv[])
    {
        InitializeCriticalSection(&cs);

        HANDLE hThread = CreateThread(NULL, 0, ::IdleThread, NULL, 0, NULL);

        printf("Are you want to quit? You type 'Q' or 'q'. If you type other key, Sound Plays is Loop.\n");

        while(1)
        {
            EnterCriticalSection(&cs);

            char key = getchar();

            if(key == 'q' || key == 'Q')
            {
                LeaveCriticalSection(&cs);
                break;
            }

            LeaveCriticalSection(&cs);

        }

        if(hThread != INVALID_HANDLE_VALUE)
        {
            WaitForSingleObject(hThread, 1000);

            CloseHandle(hThread);
        }

        DeleteCriticalSection(&cs);
        return 0;
    }

 

main스레드의 getchar()가 리턴되기 전까지, IdleThread의 sndPlaySound()가 실행 될 수 없고, IdleThread의 sndPlaySound()가 리턴 될 때 까지 main스레드의 getchar()가 실행 될 수 없습니다.

동기화 객체의 사용이 끝났다고 알려주기 전까지는 다른 스레드는 대기 상태에 빠질 수 밖에 없습니다. 현재는 두개의 스레드이기 때문에 '아...상호 연관 데이터가 없네? 그럼 크리티컬 섹션을 쓰지 말자'라거나, '블럭함수니까 크리티컬 섹션으로 감싸면 안되겠구나' 라는 판단을 바로 내릴 수 있지만, 스레드가 늘어나고 로직이 복잡해지면 이런 판단을 쉽게 내리기 어려워집니다.

그래서 멀티 스레드에 대한 고민은 코드 작성에서 중요한 부분을 차지하고, 동기화에 실수가 생겼을 때 파장이 크기 때문에 더 신중 해야 합니다.

하지만 이보다 더 큰 문제는 데드락 입니다. 수행 속도 저하는 어쨋거나 코드가 정상적으로 수행은 됩니다. 하지만, 데드락의 경우는 프로그램이 아예 정지해 버리죠.
아! 데드락이 뭐냐고요? 데드락이란, 한글로는 교착 상태라고 표현하곤 하는 상태를 말하는데요, 크리티컬 섹션 객체를 사용하는 동기화를 설명 드릴 때, ‘내가 이 데이터 사용을 끝내기 전에 다른 스레드는 사용하지마’라는 의미를 가진다고 말씀 드렸죠? 반대로 자신의 관점에서도 어떤 스레드가 내가 사용하려는 데이터를 사용 중이라면, 사용을 끝낼 때까지 기다려야 합니다.

 

그런데 만일 대기 중인 이유가, 내가 사용중인 데이터를 기다리기 때문이라면 어떻게 될까요? 이럴 경우 서로 기다리기만 할 뿐 (상호 대기 상태라 합니다) 더 이상 아무 일도 할 수 없는 상태가 되어버리죠. 이 상태가 바로 데드락(DeadLock: 교착) 상태입니다.

그래서 데드락에 빠지지 않기 위해, 큰 작업 분류 별로 스레드를 나눈 후 블럭이 걸리는 일은 별도의 스레드를 만들어 돌리고, 각 스레드 별로 처리한 결과를 다른 스레드에 전달 하거나 받아야 할 때에만 메시지 큐 등을 통해서 전달 받아 처리하는 방법이 사용되곤 합니다.

이는 윈도우즈 API에서도 내부적으로 사용하는 방식이기도 한데요, 타이머 처리나 윈도우 메시지, 예제로 사용했던 sndPlaySound()함수의 두 번째 인수로 SND_ASYNC가 사용될 경우 백그라운드 작업을 통해 처리하고 있다가, 그 결과를 어플리케이션에 알려줘야 할 경우 메시지 큐 등을 통해 전달하는 것과 같은 방식입니다.

 

이런 처리 방식에서 핵심은 동기화가 필요한 상황 자체를 줄이는 데에 있습니다. 동기화가 필요한 상황자체가 적다면 다른 스레드를 기다려야 하는 상황/시간도 줄어들고 그로 인한 효율성 낭비가 줄어든다는 것에 기반하고 있습니다.

세상 어떤 일에도 절대, 최고의 방법은 존재하지 않습니다. 제가 설명 드린 방식보다 더 좋은 멀티 스레드 활용법에 대해 한번쯤 고민 해보시는 건 어떨까요?