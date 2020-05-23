# String Pool

Java는 String Pool을 통해 String 객체들을 관리한다. String 자료형은 Immutable 자료형이다.

아래와 같이 Java에서 array 두개를 비교해보면 같은 값을 가졌더라도 `==` 연산을 해보면 `false`가 나온다. `==`은 배열의 주소를 비교하는 연산이고 같은 값을 가지는 두 array의 주소가 다르기 때문이다.

```java
int[] int_arr1 = {1,2,3};
int[] int_arr2 = {1,2,3};
int[] int_arr3 = int_arr1;
System.out.println(int_arr1 == int_arr2);
System.out.println(int_arr1 == int_arr3);

// 실행결과
// false
// true
```
하지만 String의 경우는 다음과 같다.

```java
String a = "asdf";
String b = "asdf";

System.out.println(a == b);

// 실행결과
// true
```

String은 Char의 배열인데 어떻게 각각 생성한 String의 주소가 같다고 나올 수 있을까? 이것은 JVM이 String Pool을 가지고 있기 때문이다.

String Pool에 대해 자세히 알아보자. 우선 `String a = "asdf"` 이렇게 String 객체를 생성할 때 JVM은 String Pool이라는 공간에서 `asdf`와 같은 값이 있는지 equal 연산을 통해 찾아본다. 만약 있으면 그 주소값을 그대로 return하고 그렇지 않으면 `asdf`를 String Pool에 추가한 후 그 주소값을 return하게 된다.

이렇게 String 객체를 생성할 때 바로 생성하지 않고 String Pool에서 같은 값을 찾아 return해 주는 것을 컴퓨터공학에서는 `String Interning`이라고 한다. 그러므로 String Pool 이라는 것은 JVM이 String Interning을 구현한 것이라고 할 수 있다.

Java에서는 String을 String Pool에 등록할 수 있는 메서드인 intern() 메서드를 지원한다. 모든 String 객체는 intern() 메서드를 가지고 있는데 `a.intern()`을 실행 시키면 `a` 객체가 가지고 있는 값이 String Pool에 있는지 검색한 후 있으면 그 주소값을 return하고 없다면 String Pool에 등록 후 주소값을 return한다.

그런데 `String a = "asdf"`와 같이 스트링 객체를 생성할 때는 내부적으로 intern() 메서드를 실행시켜 객체를 생성하는 시점부터 String Pool을 뒤져서(검색해서) 가져온다. 그러므로 위에서 생성한 두 String의 `==`연산 값이 true가 나오게 되는 것이다.

한편, 값이 같은 String 객체의 주소가 다른 경우를 살펴보자.

```java
String a = "asdf";
String b = new String("asdf");

System.out.println(a == b);
System.out.println(a.equals(b));
// 결과
// false
// true
```

위와 같이 String 객체를 `String b = new String("asdf")`와 같이 new를 사용해서 생성하게 되면 이것은 새로운 객체 생성을 강제하는 표현이다. 이런 경우 String Pool을 참조하지 않기 때문에 주소값이 달라지게 되므로 `==`연산 결과는 false로 나오게 된다.

하지만 equals() 메서드는 char를 하나씩 비교해가며 같은 값인지 판별하기 때문에 `a.equals(b)`의 값은 true로 나오게 된다. equals() 메서드를 사용하지 않고 a 와 b 가 

```java
String a = "asdf";
String b = new String("asdf");

System.out.println(a == b.intern())
```