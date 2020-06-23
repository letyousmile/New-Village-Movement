# JavaScript

##### THIS

- 모든 함수가 실행될 때마다 함수 내부에 this 라는 객체가 추가됨

- Arguments 라는 유사 배열 객체와 함께 함수 내부로 암묵적으로 전달됨

- 상황

  1. 객체의 메서드를 호출할 때

     - 객체의 프로퍼티가 함수일 경우 메서드라고 부름

     - this는 함수를 실행할 때 함수를 소유하고 있는 객체(메소드를 포함하고 있는 인스턴스)를 참조한다. 즉 해당 메서드를 호출한 객체로 바인딩됨

     - A.B 일 때 B 함수내부에서의 this는 A를 가리키는 것이다

     - ```javascript
       var myObject = {
         name: "foo",
         sayName: function() {
           console.log(this);
         }
       };
       myObject.sayName();
       // console> Object {name: "foo", sayName: sayName()}
       ```

     - 프로퍼티로 부르니 그 객체를 부름

  2. 함수를 호출할 때

     - 특정 객체의 메서드가 아니라 그냥 함수를 호출

     - 함수 내부 코드에서 사용된 this는 전역객체에 바인딩 됨

     - A.B일 때 A가 전역객체가 되므로 B함수 내부에서의 This는 당연히 전역 객체에 바인딩

     - ```javascript
       var value = 100;
       var myObj = {
         value: 1,
         func1: function() {
           console.log(`func1's this.value: ${this.value}`);
       
           var func2 = function() {
             console.log(`func2's this.value ${this.value}`);
           };
           func2();
         }
       };
       
       myObj.func1();
       // console> func1's this.value: 1
       // console> func2's this.value: 100
       ```

     - func1 은 myobj의 프로퍼티 이므로 this 는 그 객체가 됨

     - func2는 그냥 함수 호출이므로 애는 전역의 this가 됨

  3. 생성자 함수를 통해 객체를 생성할 때

     - new 키워드를 통해서 호출된 함수 내부에서의 this는 객체 자신이 된다

     - new 연산자를 통해 함수를 생성자로 호출하게 되면 일단 빈 객체가 생성되고 this가 바인딩 됨

     - ```javascript
       var Person = function(name) {
         console.log(this);
         this.name = name;
       };
       
       var foo = new Person("foo"); // Person
       console.log(foo.name); // foo
       ```

     - 이러면 this에서 자기 객체가 됨

  4. apply, call, bind를 통한 호출

     - ```javascript
       var value = 100;
       var myObj = {
         value: 1,
         func1: function() {
           console.log(`func1's this.value: ${this.value}`);
       
           var func2 = function(val1, val2) {
             console.log(`func2's this.value ${this.value} and ${val1} and ${val2}`);
           }.bind(this, `param1`, `param2`);
           func2();
         }
       };
       
       myObj.func1();
       // console> func1's this.value: 1
       // console> func2's this.value: 1 and param1 and param2
       ```

     - 바인드는 this와 파라미터를 지정해줄 수 있으며, call과 apply는 함수를 호출할 때 this와 파라미터를 지정해준다.

