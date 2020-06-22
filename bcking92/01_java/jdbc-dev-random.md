# JDBC-DEV-RANDOM
가끔씩 배치프로그램이 알 수 없는 이유로 종료되는 경우가 있었다. 오류로그를 보면 Oracle connection reset이나 socket exception 오류 등이 발생했다. 공통점은 Oracle DB와 커넥션을 연결하지 못했다는 것인데 여기에는 JDBC 드라이버가 어떻게 DB와 커넥션을 만드는지 알아볼 필요가 있었다.

JDBC 드라이버는 connection을 만들기 위해 connect string을 암호화 한다. connect string을 암호화하기 위해서는 난수가 필요한데 Linux 환경에서는 /dev/random를 통해 생성된 난수를 사용한다.

/dev/random이 뭘까?

Linux에서 난수를 만들어내는 곳은 두가지가 있다.
1. dev/random
2. dev/urandom

먼저 dev/random은 I/O 장치의 노이즈(디스크 읽기, 네트워크 패킷, 키보드와 마우스 입력 등)을 수집하여 entropy pool에 저장하고 이 값을 이용해서 난수를 만들어낸다. 그런데 dev/random은 일정 수준이상의 entropy가 있어야 난수를 만들어 낼 수 있는 제약이 걸려있다. 그래서 entropy가 충분하지 않다면 block대기를 해버리는데 이것 때문에 JDBC에서 커넥션에러를 내는 것이었다.

그럼 dev/urandom을 뭘까? u random, 즉 unlimited random. 난수를 생성하는데 제약조건이 없다. 적은 수의 entropy여도 바로 난수를 생성하며 block대기는 경우가 없다.

그럼 dev/random 과 dev/urandom의 차이가 뭘까? 엔트로피라는 것은 난수를 생성함에 있어서 얼마나 고른(예측불가능한)난수를 생성할 수 있을지를 말한다. 그러므로 dev/random이 보안적인 면에서는 dev/urandom보다 뛰어날 수 있지만 일반적인 경우는 난수의 질이 큰 차이를 만들어내지도 않고 치명적으로 block대기 방식 때문에 dev/random 보다는 dev/urandom을 많이 사용한다고 한다.

그럼 JDBC 드라이버가 난수생성을 /dev/urandom으로 하게하기 위해선 어떻게 설정해야 할까

`$JAVA_HOME/jre/lib/security/java.security`에 

```txt
securerandom.source=file:/dev/urandom
```
으로 바꿔주면 된다. 혹은 WAS로 Jeus를 사용하고 있다면

`$JEUS_HOME/config/<hostname>/JEUSMain.xml` 컨테이너 별 커맨드 옵션에 -Djava.security.egd=file://dev/urandom 을 추가하면 된다.