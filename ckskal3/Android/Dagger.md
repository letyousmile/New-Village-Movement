## Dagger

> ### DI & IoC
>
> **DI(Dependenct Injetion) Framework** 이다. **DI** 즉 의존성 주입이란 외부에서 의존 객체를 생성하여 넘겨주는 것을 의미한다.		
>
> 예를 들면 
>
> ```java
> class A {
> 	B b = new B();
> 	C c = new C();
> }
> ```
>
> 보통은 위와 같이 외부 클래스를 생성하는 의존 형태이다
>
> ```java
> @Module
> class Module {
> 	@Provides
>   	B provideB(){
>   	return new B();
> 	}
> }
> ```
>
> ```java
> @Component(module = Module.class)
> interface Component {
> 	void inject(A a);
> }
> ```
>
> ```java
> class A{
> 	@Inject
> 	B b;
> 	
> 	void setB(){
> 		Component component = DaggerComponent.bulider()
> 		.module(new Module())
> 		.build();
> 		
> 		component.inject(this);
> 	}
> }
> ```
>
> 위와 같이 Class A 안에서 new 로 B를 생성 하지 않아도  inject를 통해 B를 생성할 수 있다. 이런 형태를 DI 즉 의존성주입이라 한다.
>
> 즉 **DI** 를 위해서는 객체를 생성하고 넘겨주는 외부의 무언가가 필요하다. 이를 DI Framework가 하는 일이다. 스프링에서는 Container 가 전담하며 **Dagger**에서는 **Component** + **Module** 이 전담한다. DI는 의존성이 있는 객체의 제어권을 외부에 두면서 **IoC(Inversion of Control, 제어의 역전)** 개념을 구현한다.
>
>
>
> DI는 IoC 구현 방법들 중 **"한가지"** 이다.
>
> ### DI 가 필요한 이유
>
> - 의존성 파라미터를 생성자에 작성하지 않으므로 보일러 플레이트 코드를 줄일 수 있다.
> - Interface에 구현체를 쉽게 교체하면서 상황에 따라 적절한 행동을 정의할 수 있다.
>
> ### Dagger의 기본 5가지 개념
>
> - **Inject**
>   - 의존성 주입을 요청하는 부분, 어노테이션을 통해 주입을 요청하면 연결된 **Component**가 **Module**로 부터 객체를 생성하여 넘겨준다.
> - **Component**
>   - 연결된 **Module**을 이용하여 의존성 객체를 생성하고, **Inject**로 요청받은 인스턴스에 생성한 객체를 주입한다.  **Dagger**에서 주된 역할을 한다.
> - **SubComponent**
>   - **Component**는 계층구조를 가질수 있는데, 이때 **Component**의 Inner Class 방식의 하위 계층의 **Component**이다. **Sub**의 **Sub**도 가능하다. **Inject**로 주입을 요청받으면 먼저 **SubComponent**에서 의존성을 검색하고 부모로 올라가면서 검색한다.
> - **Module**
>    - **Component**에 연결되어 의존성 객체를 생성한다. 생성 후 **Scope**에 따라 관리 한다.
> - **Scope**
>   - 생성된 객체의 **Lifecycle**의 범위이다. 안드로이드에서는 주로 Activity, Fragment 등으로 화면의 생명주기와 맞추어 사용한다. **Module**에서 **Scope**을 보고 객체를 관리한다.
>
> 즉 **Module**에서 공급할 객체를 생성하고,  **Component** 를 통해 어디에 주입할지 정한 후 **Inject**를 통해 해당 위치에 해당 객체를 주입해주는 형태이다.
>
>  [자세한 부분은 여기서](https://medium.com/@maryangmin/di-%EA%B8%B0%EB%B3%B8%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B9%8C%EC%A7%80-dagger2-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-3332bb93b4b9)