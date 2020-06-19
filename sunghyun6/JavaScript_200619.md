# JavaScript

##### JavaScript Event Loop

- 싱글스레드 기반으로 동작하는 자바스크립트

- 이벤트 루프를 기반으로 하는 싱글 스레드 Node.js

- 자바 스크립트 엔진이란 자바스크립트를 해석하고 실행하는 인터프리터

- 엔진은 크게 세 영역으로 나뉨

  - Call stack
    - 단 하나의 호출 스택을 사용함 자바 스크립트는
    - 함수가 실행되는 방식이 Run to Completion
    - 하나의 함수가 실행되면 끝날 때까지 다른 어떤 task도 수행될 수 없다
    - 순차적으로 호출 스택에 담아 처리함
  - Task Queue(Event Queue)
    - 런타임 환경에서 처리해야하는 Task들을 임시 저장
    - 콜스택이 비어졌을 때 먼저 대기열에 들어온 순서대로 수행
  - Heap
    - 동적으로 생성된 객체(인스턴스) 가 힙에 할당됨

- 여기에 Event Loop 이 Task Queue에 들어가는 task를 관리한다

  - 이벤트, 콜백 등 여러 애들은 콜스택이 비어야 걔를 콜스택에 넣음

  ```javascript
  var fs = require('fs');
  
  function executeCallbacks() {  
      fs.readdir('.', function (err, filenames) {
          var i;
          for (i = 0; i < filenames.length; i++) {
              fs.stat('./' + filenames[i], function (err, stats){
                  console.log(i + ':'+stats.isFile());
              });
          }
      });
  }
  
  executeCallbacks();  
  ```

이걸 실행하면 만약 파일길이가 5면 5만 계속 나옴..

첨에 콜백하나를 이벤트큐에넣고 콜스택 종료됨

그럼 이벤트큐에서 하나 꺼내서 콜스택에 넣음

그럼 애가 포문 돌면서 이벤트큐에 콜백 5개 다 넣음

콜스택 비워졌으니 다시 이벤트큐에서 콜백 꺼냄

앤 지금 i가 5라서 5 출력함..

```javascript
var fs = require('fs');

function executeCallbacks() {  
    fs.readdir('.', function (err, filenames) {
        var i;
        for (i = 0; i < filenames.length; i++) {
            (function(){
                var j = i;
                fs.stat('./' + filenames[i], function (err, stats){
                    console.log(j + ':'+stats.isFile());
                });
            })();
        }
    });
}

executeCallbacks();  
```

앤 Closure를 생성하여 새로운 scope를 만들었고, 이 scope에 변수값을 저장해놓고 내부 콜백시 j를 출력하니 j가 순차적으로 잘 출력됨