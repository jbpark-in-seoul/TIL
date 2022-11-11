## PSA(Portable Service Abstraction)
### PSA란?
* 서비스 추상화를 통해 특정 환경이나 서버, 기술에 종속되지 않고 개발자에게 편의성을 제공해주는 것을 말한다.

### 예시
* `Spring Web MVC`
  * `@Controller` 어노테이션을 사용하면 요청을 매핑할 수 있는 클래스가 된다.
    * `@GetMapping`, `@PostMapping`등의 어노테이션을 사용해서 요청을 매핑할 수 있다.
  * 개발자는 `HttpServlet`을 상속 받고 `doGet()`, `doPost()`를 구현하지 않아도 된다.
  * 특정 기술, 환경에 종속적이지 않다.
    * `spring-boot-starter-web` 의존성 대신 `spring-boot-starter-webflux` 의존성을 받도록 바꿔주기만 하면 톰캣이 아닌 netty 기반으로 실행하게 할수도 있다. 

* `Spring Transaction`
  * `@Transactional` 어노테이션을 사용하면 간편하게 트랜잭션 처리가 이루어진다.
    * `low level`로 트랜잭션 처리를 하려면 `setAutoCommit()`과 `commit()`, `rollback()`을 명시적으로 호출해야 한다.

* `Spring Cache`
  * `@Cacheable` 어노테이션을 사용하여 구현체에 의존하지 않고 캐시 기능을 사용할 수 있다.
    * `JCacheManager`, `ConcurrentMapCacheManager`, `EhCacheCacheManager`와 같은 여러가지 구현체를 사용할 수 있다.
