# Homebrew
Homebrew는 터미널에서 명령을 실행하는 것으로 패키지 설치 및 제거를 할 수 있는 Mac용 패키지 관리 도구이다. 

Homebrew의 기본 명령어는 `brew`이다. 
```shell
$ brew --version
Homebrew 2.4.1
Homebrew/homebrew-core (git revision 94af79; last commit 2020-06-22)
```

- 패키지 설치
    ```shell
    $ brew install 패키지 이름
    ```

- 패키지 제거
    ```shell
    $ brew uninstall 패키지 이름
    ```

- Homebrew 업데이트
    ```shell
    $ brew update
    ```

- 패키지 업데이트
    ```shell
    # 설치된 모든 패키지를 업데이트
    $ brew upgrade
    # 설치된 특정 패키지만 업데이트
    $ brew upgrade 패키지 이름
    ```

- 설치된 패키지 목록 보기
    ```shell
    $ brew list
    ```

- 패키지 검색
    ```shell
    $ brew search 패키지명
    ```