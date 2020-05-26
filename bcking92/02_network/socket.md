# Socket
- Socket: 프로그램이 네트워크에서 데이터를 송수신할 수 있도록, "네트워크 환경에 연결할 수 있게 만들어진 연결부"

Socket은 보통 OSI 7 Layer(Open System Interconnection 7 Layer)의 네 번째 계층인 TCP(Transport Control Protocol) 상에서 동작하는 소켓을 주로 사용하는데 이를 TCP socket 또는 TCP/IP Socket이라고 한다.(UDP에서 동작하는건 UDP 소켓)

## Socket 통신 vs HTTP 통신
HTTP 프로토콜을 이용한 통신은 정적(stateless)이다. 클라이언트가 서버에 요청을 보내면 서버는 요청에 맞는 응답을 보내주고 연결이 끊어진다. 그런데 계속해서 데이터를 주고받고 싶을 땐 어떻게 해야할까? 만약 HTTP 통신을 이용한다면 데이터를 주고 받는 동안에 엄청나게 많은 연결요청을 보내게 될 것이다. 이건 좀 비효율적인데 그래서 등장한 것이 소켓통신이다. 

소켓통신은 HTTP 통신과 달리 두 개의 프로그램이 서로 연결되면 연결을 일부러 끊기 전까지 연결을 유지할 수 있다. 연결이 유지된 동안은 데이터 송수신이 가능하며 스트리밍, 채팅, 온라인게임등 실시간 통신이 필요한 상황에 강점이 있다.

또한 소켓 통신은 클라이언트가 요청을 보내는 경우에만 서버가 응답하는 HTTP와는 달리 클라이언트와 서버가 연결되어있으면 서버에서도 클라이언트로 데이터를 자유롭게 보낼 수 있다.

# Socket Programming
Socket을 사용하여 네트워크 통신을 구현하는 과정

## Clinet Socket & Server Socket

두 개의 프로그램이 소통하기 위해서는 한쪽은 연결(Connection)을 기다려야하고 한쪽은 연결을 요청해야 한다. 이 때, 

- 연결을 요청하는 Socket을 `Clinet Socket`이라고 하고
- 연결을 기다리는 Socket을 `Server Socekt`이라고 한다.

이렇게 역할이 나누어져 있지만 실은 Socket이라는 것은 하나의 개념이다. 다만 역할에 따라 두 프로그램간 네트워크가 연결(Connection)되기 위해서는 IP와 Port 번호가 필요하다.


## Socket API flow
- Client Socket
    1. 소켓 생성(create)
    2. 연결 요청(connect)
    3. (서버에서 소켓을 받아들이면) 송수신(send/recv)
    4. 소켓 닫기(close)

- Server Socket
    
    1. 소켓 생성(create)
    2. 서버가 사용할 IP와 PORT를 소켓에 결합(bind)
    3. 클라이언트로부터 요청이 수신되는지 주시(listen)
    4. 요청을 받아들이고(accept) 데이터 송수신을 위한 소켓을 생성.(**주의** 소켓을 하나 더 생성하는것임. 새로생성된 이 소켓을 통해 데이터 송수신!)
    5. 새로운 소켓을 통해 연결을 수립(ESTABLISHED)되면 데이터를 송수신(send/recv)
    6. 소켓 닫기(close)

## Socket Programming in Java
TCP Socket을 이용한 통신을하는 프로그램을 Java로 만들어보기 

- Server Socket

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class MainServer {
    public static void main (String[] args) {
        try {
            ServerSocket s_socket = new ServerSocket(8080);
            // 서버 소켓을 생성(create) 후 IP는 내 ip, port는 8080에 결합

            Socket c_socket = s_socket.accept();
            // 요청이 수신되는지 대기하다가(listen) 요청을 받아들이고(accept)
            // client와 데이터를 주고받을 새로운 소켓을 생성(c_socket)

            OutputStream output_data = c_socket.getOutputStream();
            // client와 연결된 socket으로 부터 OutputStream 객체를 가져옴
            // 이 객체를 이용해 client에게 메세지를 보내게된다.
            // 메세지를 받는것을 getInputStream()
            
            String sendDataString = "Welcome to My Server";
            output_data.write(sendDataString.getBytes());
            // Client와 연결된 socket의 OutputStream 객체의 write 메서드를 통해 데이터를 보낸다.(send)

            s_socket.close();
            c_socket.close();
            // 만들었던 소켓을 닫는다.(close)
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- Client Socket

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.Socket;

public class MainClient {
    public static void main (String[] args) {
        try {
            Socket c_socket = new Socket("127.0.0.1", 8080);
            // 소켓을 만들고(create) ip와 port를 이용해 해당하는 server socket에 연결(connect)을 요청

            InputStream input_data = c_socket.getInputStream();
            // 연결이 되었다면 getInputStream을 통해 들어오는 데이터를 받는 객체를 생성한다.

            byte[] receiveBuffer = new byte[100];
            input_data.read(receiveBuffer);
            // 바이트 형태로 전송되는 데이터를 받을 receiveBuffer를 만들고 
            // InputStream 객체의 read 메서드를 이용해 데이터를 receiveBuffer에 담아준다.

            System.out.println(new String(receiveBuffer));
            // 받은 메세지 출력

            c_socket.close();
            // 소켓을 닫는다.(close)
        }
    }
}
```

# 참고

- https://m.blog.naver.com/highkrs/220840680504
- https://mangkyu.tistory.com/48