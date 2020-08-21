# Shell Script Programming
## Kernel & Shell
### Kernel(커널)
커널은 하드웨어와 어플리케이션 간에 상호 작용을 도와주는 OS의 핵심 구성요소.
리눅스의 커널 버전을 확인하는 방법
```shell
$ uname -r
```
### Shell(쉘)
유닉스 계열 시스템에서 사용하는 대화형 인터페이스. 사용자와 커널 사이에서 사용자의 입력을 받아 명령을 해석하여 커널에 전달하고 결과를 사용자에게 반환함.

가장 인기있는 5가지 쉘은 Bash, Tcsh, Ksh, Zsh, Fish.

- 현재 사용중인 쉘 확인
    ```shell
    $ echo $SHELL
    ```
- 사용가능한 쉘 확인
    ```shell
    $ cat /etc/shells
    ```

## Process(프로세스)
프로세스는 실행중인 프로그램을 말한다. 여러개의 프로그램을 동시에 실행하는 것을 멀티태스킹이라고 하고 프로세스들을 관리하는 것이 운영체제의 주요 역할중 하나이다.

### 프로세스 실행 방식
리눅스의 프로세스는 foreground와 background방식으로 동작한다. foreground방식은 키보드 입력을 받아서 동작하는 방식으로 실행되며 다른 작업을 처리할 수 없다. background방식은 사용자와의 대화 없이 실행되는 작업 방식이다.

리눅스에서 foreground 방식으로 작업을 실행하면 다른 작업을 실행할 수 없고 작업이 끝날때까지 대기한다. 백그라운드 방식은 명령어 뒤에 `&`를 붙여 실행하고, 다른 명령어를 추가적으로 입력할 수 있다.

background방식으로 실행해도 사용자의 터미널 세션이 종료되면 실행중인 프로세스도 종료된다. 작업시간이 오래 걸리는 작업인 경우 `nohup`(no hang up) 명령어를 이용하여 백그라운드로 실행하면 사용자의 터미널 세션이 종료되어도 작업이 종료될 때까지 프로세스를 실행한다.

```shell
# foreground (서버 파일이 실행되면 ctrl + c 로 종료하기 전까지 다른 입력 불가)
$ python app.py

# background (서버 파일이 백그라운드에서 실행되므로 다른 명령어 입력가능 하지만 terminal에서 exit하면 프로세스가 종료됨)
$ python app.py &

# nohup (서버 파일이 백그라운드에서 실행되며 terminal에서 exit하여도 프로세스가 계속 실행됨)
$ nohup python app.py &
```


## Commands(명령어)
### System Management
- [`crontab`](./Cron.md) 링크참조
- `free`
    - 메모리 사용량을 확인
        ```shell
        $ free
        $ free -h #단위를 읽기 편하게 바꿔서 출력
        $ free -s 2 #2초마다 메모리 사용량 출력
        ```

    - total: 전체 메모리 용량
    - used: 사용중인 메모리 용량
    - free: 유휴 메모리 용량
    - shared: 공유 메모리 용량, 프로세스, 스레드간 통신을 위해 사용
    - buffers: 버퍼 메모리 용량. 파일 저장을 위한 임시 저장공간 등
    - cached: 캐쉬 메모리 용량. 자주 사용하는 데이터를 메모리에 캐싱하여 IO 속도 증가

- `jobs`
    - 현재 계정에서 **실행**중인 작업을 표시
        ```shell
        $ jobs
        $ jobs -l #프로세스 ID를 표시
        ```
    - Running: 실행중
    - Stopped: 일시 중단(Ctrl + Z)
    - Terminated: 강제 종료(kill로 종료)
    - Done: 정상종료

- `kill`
    - 프로세스 종료
    - `kill` 명령어는 프로세스에 시그널을 전송한다. 시그널을 생략하면 TERM 시그널을 전송하여 프로세스를 종료한다. -9 옵션을 이용하여 프로세스를 강제종료할 수 도 있다.

    - 시그널
        - 1: HUP, 프로세스에 재기동을 통지
        - 2: INT, 프로세스에 인터럽트를 통지
        - 3: QUIT, 프로세스에 종료를 통지
        - 9: KILL, 프로세스에 강제종료를 통지
        - 15: TERM, 프로세스에 종료를 통지
        - 17: STOP, 프로세스에 중단을 통지
        - 19: CONT, 프로세스에 재개를 통지

    ```shell
    $ kill -9 %1 # job ID를 이용한 종료
    $ kill -9 14320 # PDI를 이용한 종료
    ```

- `man`
    - 명령어의 메뉴얼을 출력
        ```shell
        $ man cp # cp명령어의 메뉴얼을 출력
        $ man crontab # crontab 명령어의 메뉴얼을 출력
        ```
- `nohup`
    - 리눅스에서 프로그램을 실행할 때 사용자의 세션이 끊어지면(hang up) 프로그램도 함께 종료된다. 처리에 오랜 시간이 걸리는 프로그램을 실행할 때 `nohup`을 이용하여 사용자의 세션이 끊어져도 프로그램은 계속 실행되도록 할 수 있다.

    ```shell
    $ nohup python app.py &  # app.py를 백그라운드에서 no hang up으로 실행하고 표준 출력을 nohup.out 으로 저장함

    $ nohup python app.py > app.log & # app.py를 백그라운드에서 no hang up으로 실행하고 표준 출력을 app.log에 저장
    ```