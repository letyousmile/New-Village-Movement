# Cron(크론)

크론은 유닉스 계열 운영체제에서 정해진 스케줄에 작업을 실행하기 위해 사용하는 데몬이다.

crontab 파일은 특정 시간에 실행되어야 할 커맨드 리스트가 포함되어있는 text 파일이다. 이 파일은 `crontab` 커맨드로 편집할 수 있다. Cron 데몬은 crontab 파일의 커맨드를 체크하여 백그라운드에서 실행한다.

각 유저(root 포함)은 crontab 파일을 가지고 있다. Cron 데몬은 유저의 로그인 여부에 상관없이 유저들의 crontab 파일을 체크한다.

아래 명령어로 help를 볼 수 있다.
```shell
man crontab
```

## Cron 사용하기

현재 로그인한 유저에 대해 cron을 등록할 때:
```shell
crontab -e
```

root 권한으로 실행해야 되는 파일이 있을 때 cron을 등록하려면:
```shell
sudo crontab -e
```

## Crontab Lines

Crontab 파일에서 각각의 줄은
```crontab
01 04 1 1 1 /usr/bin/somedirectory/somecommand
```
의 형태로 되어있다. 앞의 5개의 숫자가 의미하는 것은 차례대로 (분, 시간, 일, 월, 요일) 이고 뒤에는 실행시킬 command를 적는다.

예를들어 위에 적힌 것의 의미는 매년 1월 1일 4시 1분 & 매년 1월의 월요일 4시 1분에 해당 커맨드를 실행하라는 뜻이다.

값의 범위는 분(0~59), 시간(0~23, 0=자정), 일(1~31), 월(1~12), 요일(0~6, 0=Sunday)이다.

이러한 값에는 asterisk(*)도 사용될 수 있는데 의미는 매(분,시간,일,월)이다. 

```crontab
01 04 * * * /usr/bin/somedirectory/somecommand
```
예를 들어, 위의 커맨드는 매일 4시 1분에 실행된다.

comma(,)와 dash(-)도 시간표현에 사용될 수 있다.

```crontab
01,31 04,05 1-15, 1,6 * /usr/bin/somedirectory/somecommand
```
예를 들어, 위의 커맨드는 매년 1월, 6월의 1일 부터 15일까지 4시 01분, 31분, 5시 01분, 31분에 실행된다.

만약 매분, 매시간, 매일, 매월 말고 특정 시간마다 커맨드를 실행하고 싶다면 slash(/)를 사용할 수 있다.

```crontab
*/10 * * * * /usr/bin/somedirectory/somecommand
```
위의 명령어는 아래의 명령어와 같다.

```crontab
0,10,20,30,40,50 * * * * /usr/bin/somedirectory/somecommand
```

그 밖에도 아래와 같은 표현을 사용할 수 있다.
|string|meaning|
|-|-|
|@reboot|Run once, at startup.|
|@yearly|Run once a year, "0 0 1 1 *".|
|@annually|(same as @yearly)|
|@monthly|Run once a month, "0 0 1 * *".|
|@weekly|Run once a week, "0 0 * * 0".|
|@daily|Run once a day, "0 0 * * *".|
|@midnight|(same as @daily)|
|@hourly|Run once an hour, "0 * * * *".|

e.g.
```crontab
@reboot /path/to/execuable1
```
그 이외의 다양한 시간 표현에 대해 궁금하다면:
```shell
man 5 crontab
```

## Options
1. `crontab -l` 현재 crontab을 출력
2. `crontab -r` 현재 crontab 제거
3. `crontab -e` 현재 crontab을 편집 (EDITOR 환경변수에 지정된 에디터를 사용한다.)

## Allowing/Denying User-Level Cron

유저를 허가하는 것은 /etc/cron.allow 파일에 유저를 적어놓음으로써 가능하다.

유저를 금지하는 것은 /etc/cron.deny 파일에 유저를 적어놓음으로써 가능하다.

만약 이 두파일이 모두 없다면 crontab에 있는 모든 job이 실행된다.(어떤 유저의 것이지 관계없이)

ubuntu에서는 이런 파일들이 기본적으로 만들어져 있지 않으므로 설정을 추가하고 싶다면 파일을 생성하면 된다.

note
- /etc/shadow 에 없는 유저는 유저의 crontab 또한 없다. 만약 유저가 /etc/passwd 에는 있고 /etc/shadow 에는 없다면 /etc/shadow 에도 등록을 해줘야 crontab을 사용할 수 있다.

## Further Considerations

명령에 따라 루트 사용자의 crontab 맷 윗줄에 다음과 같이 PATH를 추가해줘야 할 수도 있다.
```crontab
PATH=/usr/sbin:/usr/bin:/sbin:/bin
```

crontab -e는 EDITOR 환경변수를 사용한다. 그러므로 아래와 같이 기본 에디터를 바꿀 수 있다.
```crontab
export EDITOR=nano
```

## Logging

기본적으로 log는 /var/log/syslog 에 쌓이므로 크론 job이 정상적으로 실행되지 않았다면 이곳을 가장 먼저 체크해 보아야 한다.

하지만 실제 job에서 남기는 로그는 기록되지 않으므로 아래와 같이 따로 기록해 주어야 한다.
```crontab
0 0 * * * /some/job > ~/log/job_`date +\%Y\%m\%H\%M\%S`.log 2>&1
```

## Common Problem

- `%` 문자는 crontab에서 개행문자로 사용되므로 표시하고 싶다면 `\%` 와 같이 사용해야 한다.

- 아래와 같이 시간필드 뒤에 유저를 특정하게 지정해 주면 crontab을 만든 유저에 관계없이 지정된 유저의 권한으로 job을 실행하므로 에러가 날 수 있다.
    ```crontab
    0 0 * * * root /some/job
    ```

- 실수로 사용한 `crontab -r` 명령어로 인해 crontab이 지워지는 것 방지할 수 있을까?
    
    다음과 같이 shell wrapper를 이용해 -r 명령어를 차단하기
    ```.bashrc
    crontab () { [[ $@ =~ -[iel]*r ]] && echo '"r" not allowed' || command crontab "$@" ;}
    ```