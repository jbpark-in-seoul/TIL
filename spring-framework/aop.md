## AOP(Aspect Oriented Programming)
### AOP란?
* 관점 지향 프로그래밍이라고 한다.
* 어떤 로직을 기준으로 `핵심 비즈니스 관점`과 `부가적인 관점`을 구분하여 관점을 기준으로 모듈화하는 것을 말한다.
  * 핵심적인 관점: 개발자가 만들고자 하는 핵심 비즈니스 로직.
  * 부가적인 관점: 핵심 로직을 수행하기 위해 필요한 로직.
    * DB Connection, Logging, File I/O, ...
```xml
<!-- maven -->
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-aop</artifactId>
</dependency>

<!-- gradle -->
<!-- implementation 'org.springframework.boot:spring-boot-starter-aop' -->
```

### 주요 개념
* `Aspect`: 흩어진 관심사를 모듈화한 것
* `Target`: `Aspect`가 적용되는 곳(클래스, 메서드, ...)
* `Advice`: `Aspect`에서 실질적인 기능에 대한 구현체
* `Join Point`: `Advice`가 `Target`에 적용되는 시점(메서드 진입 시점, 생성자 호출 시점, 필드에서 꺼내올 시점)
* `Point Cut`: `Join Point`의 상세한 스펙을 정의한 것(구체적으로 `Advice`가 실행될 시점)

### AOP 적용 방식
* Compile Time
  * 컴파일 시점에 바이트 코드를 조작하여 `AOP`가 적용된 바이트 코드를 생성하는 방식.
* Load Time
  * 컴파일이 완료된 후 클래스를 로딩하는 시점에 클래스 정보를 변경하는 방식.
* Run Time
  * `Spring AOP`가 사용하는 방식.
  * `Bean`이 만들어질 때, `Proxy Bean`을 만들어서 `Aspect` 코드를 추가하는 방식.

### Spring AOP
* `Spring AOP`는 `Proxy Pattern` 기반의 AOP 구현체이다.
* `Spring AOP`는 `Spring Bean`에만 적용 가능하다.
* `Proxy` 객체를 사용하는 것은 접근 제어 및 부가 기능을 추가하기 위해서다.
* `Spring IoC`와 연동하여 발생하는 문제를 해결하기 위한 솔루션을 제공하는 것이 목적이다.
  * 모든 `AOP`를 제공하지 않는다.
  * 발생 문제: 중복 코드, 프록시 클래스 작성의 번거로움, 객체간 관계 복잡도 증가, ...
* `Proxy Pattern`
  * 프록시는 대리자, 대변인이라는 의미를 갖고 있다.
  * 프록시 객체에게 어떤 일을 대신 시키는 것이다.
  * 어떤 객체를 직접 참조하는 것이 아닌 프록시 객체를 참조하고, 프록시 객체는 실제 객체의 반환 값을 클라이언트에 전달한다.

