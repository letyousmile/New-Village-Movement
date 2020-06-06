# Server-Start-time-out
- 에러메세지: Server Tomcat v8.5 Server at localhost was unable to start within 45 seconds. If the server requires more time, try increasing the timeout in the server editor.

서버 시작과정이 45초 이상 걸릴 때 뱉는 오류.


## 해결방법
1. 이클립스 아래쪽 서버창에서 서버를 더블클릭하면 상세정보를 볼 수 있다.

2. 상세정보에서 Timeouts으로 가면 Start와 Stop이 있는데 이 시간을 조절해서 해결하자.


# localhost-config is missing

- 에러메세지: Tomcat server configuration at \Servers\Tomcatv8.0 Server at localhost-config is missing

- 상황: 서버는 그대로 두고 project를 완전히 지웠다가 SVN에서 checkout후 maven으로 빌드하여 서버를 실행했다. 위의 메세지가 뜨면서 서버가 실행되지 않음

## 해결방법
1. 이클립스 하단에 서버를 우클릭해서 지우고 왼쪽 디렉토리 트리의 서버도 지운다.

2. 서버를 다시 만든다.