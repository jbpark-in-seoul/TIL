## IoC(Inversion Of Control)
### IoC란?
* `제어의 역전`이라는 의미로 해석이된다.
* 메소드나 객체의 호출 작업을 개발자가 제어하는 것이 아니라, 외부에서 결정되는 것을 의미한다.

### DI란?
* `DI`(Dependency Injection)는 객체를 직접 생성하는 것이 아니라, 외부에서 객체를 생헝한 후 객체에 의존성을 주입하는 것을 말한다.
* 객체가 직접 클래스를 생성하고 제어를 하면 여러 불이익이 발생한다.
```java
public class Caffe {
    
    private Americano americano;
    
    public Caffe() {
        this.americano = new Americano();
    }
}
```
* 위 코드에서 `Caffe` 클래스와 `Americano` 클래스는 높은 결합도를 갖는다.
* `Caffe` 클래스에서 추후에 `CaffeLatte`라는 클래스가 추가될 경우, `Caffe` 클래스를 직접 수정해줘야 한다.
```java
public interface Coffee {
    
}

public class Americano implements Coffee {
    
}

public class Caffe {
    
    private final Coffee coffee;
    
    public Caffe(Coffee coffee) {
        this.coffee = coffee;
    }
}
```
* 위와 같은 코드로 변경하게 되면 결합도를 낮출 수 있고, 유연성이 증가한다.
* `DI`를 사용하기 위해서는 어플리케이션 실행 시점에 `ApplicationContext`에 등록된 `Bean`을 주입해야 한다.
* 이렇게 개발자가 직접 객체의 의존관계를 제어하는 것이 아니라, 프레임워크에 제어를 위임하는 것을 `IoC`라고 한다.
* 의존성 주입 방식
  * (*)생성자 주입
  * 필드 주입
  * 수정자 주입
  * 메서드 주입
  * 의존성 방식은 `생성자 주입` 방식이 권장된다.
    1. 생성자 주입 방식은 불변성(Immutable)을 보장한다.
       * 객체가 생성될 때, 한번만 호출되므로 객체가 변하지 않는다.
    2. 테스트 코드 작성에 유리하다.
       * 테스트 코드는 특정 프레임워크에 의존 해서는 안된다.
       * 순수한 자바 코드로 테스트 코드를 작성해야 한다.
        ```java
        @Service
        public class CoffeeService {
            @Autowired
            private CoffeeRepository coffeeRepository;  // 필드 주입 방식
    
            public void register(String menu) {
                coffeeRepository.add(menu);
            }
        }
        ```
        ```java
        public class CoffeeServiceTest {
            @Test
            public void registerTest() {
                CoffeeService coffeeService = new CoffeeService();
                coffeeService.register("caffeLatte");
            }
        }
        ```
       * 위 테스트 코드는 스프링 프레임워크에서 동작하지 않으므로 의존 관계 주입이 되지 않는다. (의존 관계 주입X)
       * `수정자 주입 방식`을 사용하면 해결할 수 있으나, 불변성이 보장되지 않는다.
       * `생성자 주입 방식`을 사용하면 컴파일 시점에 객체를 주입 받아서 테스트 코드를 작성할 수 있고, 의존성 주입이 누락될 경우 컴파일 에러가 발생하여서 에러를 발견하기 쉬운 이점이 있다.
       * 
    3. Lombok 라이브러리와 결합이 가능하다.
       ```java
       @Service
       @RequiredArgsConstructor
       public class CoffeeService {
           private final CoffeeRepository coffeeRepository;
       
           public void register(String menu) {
               coffeeRepository.add(menu);
           }
       }
       ```
       * 생성자 주입을 사용하면 `final`키워드와 Lombok 라이브러리를 사용해서 위와 같이 코드를 간결하게 작성할 수 있다.
         * 생성자가 한 개인 경우 @Autowired 생략이 가능하다.
    4. 순환 참조 방지
       ```java
       @Service
       @RequiredArgsConstructor
       public class CoffeeService {
            private final BeanService beanService;
       
            public void makeCoffee() {
                ...
            }
       }
       
       @Service
       @RequiredArgsConstructor
       public class BeanService {
            private final CoffeeService coffeeService;
       
            ...
       }
       ```
       * 위 두 개의 서비스에서는 서로를 참조하면서 순환 참조가 발생한다.
       * 순환 참조가 발생하면 `StackOverFlow` 에러에 의해서 서버가 죽을 수 있다.
       * 생성자 주입 방식을 사용하면 컴파일 시점에 에러가 발생하기 때문에 사전에 방지할 수 있다.
       * `spring-boot 2.6`버젼 이후에는 필드 주입 방식에서도 순환 참조가 허용되지 않도록 변경되었다.
