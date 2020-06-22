# JavaScript

##### Hoisting

- 정의

  - hoist 는 끌어올리기 라는 뜻

  - 자바스크립트에서 끌어올려지는 것은 변수이다

  - var keyword로 선언된 모든 변수 선언은 호이스트 됨

  - ```javascript
    function getX() {
      console.log(x); // undefined
      var x = 100;
      console.log(x); // 100
    }
    getX();
    ```

  - 원래 이러면 오류가 뜰 것 같은데 안뜸. 자바스크립트는 먼저 선언을 해놓기 때문에

  - var x가 선언된 상태라서 undefined라고 뜸

  - ```javascript
    function getX() {
      var x;
      console.log(x);
      x = 100;
      console.log(x);
    }
    getX();
    ```

  - 애는 미리 선언해놨기 때문에 원래 돌아가는 방식임

  - ```javascript
    foo( );
    function foo( ){
      console.log(‘hello’);
    };
    // console> hello
    ```

  - 뒤에서 끌어올라와 지기때문에 잘 작동함

  - ```javascript
    foo( );
    var foo = function( ) {
      console.log(‘hello’);
    };
    // console> Uncaught TypeError: foo is not a function
    ```

  - 애는 함수 리터럴을 할당하는 구조라서 호이스팅이 안되서 타입에러가 뜸

##### Closure

- 클로저는 두 개의 함수로 만들어진 환경

- 환경이라 함은, 클로저가 생성될 때 그 범위에 있던 여러 지역 변수들이 포함된 문맥 그 자체

- 자바스크립트에는 없는 비공개 속성/메소드, 공개 속성/메소드를 만들 수 있음

- 조건

  - 내부함수가 익명함수로 되어 있어 외부 함수의 반환값으로 사용

  - 내부 함수는 외부 함수의 실행환경에서 실행

  - 내부 함수에서 사용되는 변수 x는 외부 함수의 변수 스코프에 있다.

  - ```javascript
    function outer() {
      var name = `closure`;
      function inner() {
        console.log(name);
      }
      inner();
    }
    outer();
    // console> closure
    ```

  - ```javascript
    var name = `Warning`;
    function outer() {
      var name = `closure`;
      return function inner() {
        console.log(name);
      };
    }
    
    var callFunc = outer();
    callFunc();
    // console> closure
    ```

  - 외부 함수 호출이 종료되더라도 외부 함수의 지역 변수 및 변수 스코프 객체의 체인관계가 유지됨

  - 즉, outer의 선언이 끝났고 callfunc를 불렀음에도 불구하고 var name = 'closure'가 유지되어 inner 함수에서 name이 클로저가 찍힘

