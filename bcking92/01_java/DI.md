# DI(Dependency Injection, 의존성 주입)
DI는 Spring에서 빼놓을 수 없는 개념이다. DI를 통해 객체간의 결합을 느슨하게 하여 코드의 유연성을 부여할 수 있다. 말만 들으면 의존성을 주입하는 것은 객체간의 결합을 느슨하게 하는 것과는 반대일 것이라고 생각하게 되는데 어떻게 그렇게 되는지 살펴보자.

어떤 클래스 A는 C라는 클래스의 객체를 가져와 사용해야 한다고 생각해보자. C는 B라는 인터페이스를 상속한다. 간단하게 A의 컨스트럭터에 C객체를 생성하는 코드를 만들어 사용할 수 있다.

```java
class A {
    private B b;

    public A() {
        b = new C();
    }
}

interface B {

}

class C implements B {

}

class D implements B {

}
```
위와 같은 코드는 A객체를 생성하면 무조건 C라는 객체가 하나 생성된다. 얼핏 보기에는 문제가 없어보인다. 
하지만 만약에 다른 파일에서 A클래스를 사용하는데 C클래스 대신 D클래스를 사용하고 싶다면 어떻게 할까? 
불가능하다. A클래스를 생성할 때 무조건 C클래스가 같이 생성되기 때문이다. 이와 같은 상태일 때 A클래스와 C클래스가 서로 결합력이 강하다고 한다.

결합력을 약하게 만들기 위해서 A클래스에 set 메서드를 추가하여 
구현한다면 아래와 같이 할 수 있다.
```java
public class Di {
    public static void main(String[] args) {
    B c = new C();
    B d = new D();
    A a = new A();
    a.setB(C);
    }
}

class A {
    private B b;

    public A() {

    }
    
    public void setB(B b) {
        this.b = b;
    }
}

interface B {

}

class C implements B {

}

class D implements B {

}
```
이런식으로 객체 C와 D를 미리 생성해 놓고 setB 메서드를 통해 C와 D를 사용한다면 편리하게 갈아 끼울 수 있다.
이것의 장점은 결합력을 약하게 해준다는 점 뿐만 아니라 매번 A 객체가 생성될 때 마다 C나 D 객체를 생성하지 않아도 된다는 점이다.

이것을 더 편하고 의미있게 사용하려면 C, D를 전역객체로 만들어 놓고 싱글톤 패턴으로 어디에서나 불러서 사용할 수 있으면 좋을 것이다.

그것을 Spring이 가능하게 해준다. 위의 코드를 Di지시서로 옮기면 아래와 같이 쓸 수 있다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
<bean id="c" class="myproject.C" />
<bean id="a" class="myproject.A" >
    <property name="b" ref="c" />
</bean>
</beans>
```
이렇게 DI 지시서를 작성하면 Spring에서는 bean객체를 만들게 되는데 bean객체는 IoC컨테이너에 담겨져 있고 전역에서 불러올 수 있다.(Application context를 통해서)

또 클래스에 setOOO으로 정의한 메서드는 property에서 set을 때고 OOO이라는 이름으로 설정할 수 있으며 아래와 같이 자료형에 따라 value와 ref를 선택하여 사용하면 된다.

```xml
<property name="setter이름" value="기본형" ref="참조형" />
```

간단하게 말하면

```xml
<property name="setter이름" value="특정값" ref="bean객체" />
```

라고 생각해도 될 것 같다.