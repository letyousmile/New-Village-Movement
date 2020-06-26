# DNS(Domain Name Server)

네임서버는 도메인에 해당하는 IP를 알려주는 서비스이다. 예를들어 브라우저에 www.naver.com을 입력하면 네임서버에서 www.naver.com에 해당하는 ip를 알려주고 해당 ip로 접속하게 되는 것이다.

보통 도메인을 판매하는 회사는 네임서버를 가지고 있다. 우리가 그 회사로부터 도메인을 구입하게 되면 해당 도메인과 IP를 매핑하는 정보를 회사의 네임서버가 가지고 있고 내 도메인으로 접속하는 사용자들은 그 네임서버에서 리턴한 IP를 통해 나의 서버로 접속할 수 있게 되는 것이다. 

그럼 매번 www.naver.com에 접속할 때 마다 네임서버에서 IP를 받아와야 하는걸까? 그렇진 않다. 한번 접속했던 도메인의 IP는 브라우저가 기억하고 있기 때문이다.

네임서버에 오류가 생기면 유저들은 IP를 통해서는 서비스에 접속할 수 있지만 도메인을 통해서는 접속할 수 없게 된다.

그렇다면 전세계에 수많은 네임서버들이 있을 텐데 어느서버에 어느 도메인이 저장되어있는줄 알고 요청을 보낼까? 

일반적으로 통신사에서 도메인과 IP 정보를 모아놓은 네임서버를 제공한다. 예를 들어 SK인터넷을 사용하고 있다면 브라우저에서 특정 도메인으로 접속할 때 SK에서 제공하는 DNS로 요청을 보내 해당 IP를 받아오는 식이다.

또한 네임서버들은 계층구조를 이루고 있다. 예를들어 www.naver.com의 IP를 찾기위해 네임서버에 요청을 보내면 네임서버는 .com의 네임서버를 참조하라고 응답하고 다시 .com의 네임서버에서는 www.naver.com의 네임서버를 참조하라고 응답하게 된다. 이런식으로 최상위 계층의 네임서버부터 타고내려가는 구조로 되어있는데 가장 최상위의 네임서버는 루트네임서버라고 불리며 전세계에 13개만 존재한다. 

실제로 루트네임서버를 통해 www.naver.com의 IP를 찾는 과정을 보고 싶다면 

[이곳](https://simpledns.plus/lookup-dg)에서 확인할 수 있다.

```txt
Sending request to "j.root-servers.net" (192.58.128.30)
Received referral response - DNS servers for "com":
-> e.gtld-servers.net (192.12.94.30)
-> b.gtld-servers.net (192.33.14.30)
-> j.gtld-servers.net (192.48.79.30)
-> m.gtld-servers.net (192.55.83.30)
-> i.gtld-servers.net (192.43.172.30)
-> f.gtld-servers.net (192.35.51.30)
-> a.gtld-servers.net (192.5.6.30)
-> g.gtld-servers.net (no IP address)
-> h.gtld-servers.net (no IP address)
-> l.gtld-servers.net (no IP address)
-> k.gtld-servers.net (no IP address)
-> c.gtld-servers.net (no IP address)
-> d.gtld-servers.net (no IP address)
Sending request to "e.gtld-servers.net" (192.12.94.30)
Received referral response - DNS servers for "naver.com":
-> ns2.naver.com (125.209.249.6)
-> ns1.naver.com (125.209.248.6)
Sending request to "ns2.naver.com" (125.209.249.6)
```

