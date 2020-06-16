# Swap Memory
Swap memory는 Linux에서 쓰이는 가상메모리이다. 어플리케이션의 RAM 사용량이 초과되면 SWAP 메모리가 사용된다. SWAP 메모리는 디스크의 일정 공간을 정해놓고 RAM 처럼 사용하는 방식으로 처리속도는 당연히 RAM보다 느리지만 어플리케이션이 물리메모리 부족으로 멈추는 현상을 방지한다.

swap 메모리를 설정할 수 있다. swap메모리를 설정하는 swap file을 만들고 아래 명령어에서 확인할 수 있다.
```shell
$ cat /proc/swaps
```
를 보면 알 수 있다.

swap file을 만드는 예시는 다음과 같다.
```shell
$ cd /var/tmp
$ dd if=/dev/zero of=swapfile1 bs=1024 count=1048576
$ dd if=/dev/zero of=swapfile2 bs=1024 count=1048576
```

### SWAP 메모리 사용량 확인하기

1. 전체 SWAP 메모리
    ```bash
    $ free
    ```
    위 명령어로 스왑 메모리 전체 사용량 확인(옵션으로는 -m -g 등등..)
2. 프로세스별 SWAP 메모리 사용율
    ```bash
    $ top
    ```
    1. top명령어 실행후 키보드 `f`키를 누르면 표시내역을 선택할 수 있는 화면으로 들어감.
    2. SWAP 이라고 표시되어있는 부분에서 키보드 `d` 혹은 `space`로 선택 후 esc로 top으로 돌아옴
    3. 맨 오른쪽에 보면 프로세스별 실시간 SWAP 메모리 사용률 확인 가능