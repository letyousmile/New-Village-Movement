# 브라우저 이벤트와 Javascript
## 브라우저 이벤트
이벤트는 무엇인가 일어났다는 신호이다. 모든 DOM 노드는 이벤트 신호를 만들어 낸다. 하지만 이벤트가 개별적인 DOM 노드에만 일어나는 것은 아니다. DOMcontentLoaded 이벤트 처럼 HTML이 로드되어 DOM 생성이 완료되었을 때 발생하는 이벤트도 있다.

## 이벤트 핸들러
이벤트에 반응하려면 이벤트가 발생했을 때 실행되는 함수를 할당해주어야 하는데 이를 이벤트 핸들러(Event Handler)라고 한다.

핸들러는 여러가지 방법으로 할당할 수 있다.
- HTML속성을 이용한 방법
    HTML element에 `on<envent>`속성에 핸들러를 할당할 수 있다.
    ```html
    <button onclick="alert('clicked')"> 
    ```
- DOM 프로퍼티에 할당하기
    DOM 프로퍼티에 `on<event>`를 사용해도 핸들러를 할당할 수 있다.
    ```html
    <button id="myBtn" onclick="alert('clicked')"> 
    <script>
     myBtn.onclick = function () {
         alert('clicked')
     };
    </script>
    ```

## 이벤트 전파
...