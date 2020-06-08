# I/O(Input/Output, 입출력)
- I/O는 Input/Output(입출력)을 의미한다. 입출력이란 프로그램이 컴퓨터 **내부 또는 외부의 장치와 데이터를 주고받는 것**을 뜻한다.

    예를들어 마우스 움직임, 키보드입력, 콘솔에 출력, 네트워크를 통한 데이터 송수신 등이 입출력에 해당한다.

    Unix에서 입출력에는 몇 가지 모델이 있다.
    - Blocking I/O
    - Non-Blocking I/O
    - I/O multiplexing(select and poll)
    - signal driven I/O (SIGIO)
    - asynchronous I/O (the POSIX aio_functions)

    Bloking model과 Non-Blocking 모델에 대해 알아보자.

    먼저, 공통적으로 입력(Input)은 일반적으로 두 개의 과정을 거치면서 일어난다.
    1. Data가 ready되길 기다린다.
    2. Data를 커널에서 copy하여 process로 전달한다.

    소켓이 동작하는 것으로 예를들어 생각해본다면, 1번 과정은 네트워크(외부)로 부터 데이터가 수신되길 기다리는 것이다. 외부에서 패킷이 도착하면 커널안에 있는 버퍼로 복사한다. 2번 과정에서는 이 버퍼에 있는 데이터를 어플리케이션 버퍼로 복사하는 것이다.

## Blocking I/O Model
I/O 작업이 진행되는 동안 프로세스가 작업을 중단한 채 대기하는 방식이다. 진행과정은 다음과 같다.
1. 프로세스는 커널에게 System call을 보내고 기다린다.
2. 커널은 return할 데이터가 올때까지 기다린다.
3. 데이터를 받은 커널은 데이터를 복사하여 프로세스에 return OK 한다.

위처럼 I/O가 진행되는 동안 프로세스는 기다렸다가 I/O가 끝나면 결과를 받아 다시 작동한다.

## Non-Blocking I/O Model
우리의 프로세스가 만약 Non-blokcking I/O를 한다고하면 그것은 
- "내가 보낸 I/O 작업이 프로세스를 멈추지 않고서는 할 수 없는 작업이면 그냥 프로세스를 멈추지말고 에러를 리턴해줘" 

라고 말하는 것과 같다. 진행과정을 보면
1. 프로세스는 커널에게 System call을 보낸다.
2. 커널은 return할 데이터가 없으므로 즉시 EWOULDBLOCK 에러를 리턴한다.
3. 프로세스는 다시 커널에게 System call을 보낸다.
4. 커널은 또 return할 데이터가 없으므로 즉시 EWOULDBLOCK 에러를 리턴한다.
5. 3, 4를 반복한다.
6. 그러다 커널에서 data가 ready된 순간 커널은 데이터를 복사하여 프로세스에 return OK 한다.

이처럼 어플리케이션이 루프를 돌면서 커널에게 System call을 계속해서 보내는 것(정확히는 어플리케이션이 recvfrom을 부르고 그게 system call을 하는것이긴함)을 Polling이라고 한다.

어플리케이션은 이렇게 커널에서 작업이 완료되었는지 확인하기 위해 계속해서 Polling을 하는데 이건 CPU 낭비로 이어질 수도 있다. 그래서 Non-blocking 모델은 하나의 기능만을 위한 시스템에서 자주 사용한다.

Nodejs의 I/O작업도 Non-blocking 모델로 이루어 지는데 이는 Nodejs 프로세스가 싱글스레드로 작동한다는 것을 최대한 활용하는 것이다. 싱글스레드 프로세스는 CPU core를 하나만 사용하므로 다른 core에게 이러한 I/O작업을 맡기는 것이 CPU 낭비로 이어지지 않고 오히려 CPU를 효율적으로 사용할 수 있게 해준다.
