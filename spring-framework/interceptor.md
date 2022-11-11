## 인터셉터(Interceptor)
### 인터셉터란?
* 인터셉터는 J2EE 표준 스펙인 필터와 달리 `스프링이 제공하는 기술`로써, `디스패처 서블릿이 컨트롤러를 호출하기 전, 후`에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공한다
* 웹 컨테이너에서 동작하는 필터와 달리 `인터셉터는 스프링 컨텍스트`에서 동작 한다.
```java
public interface HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
        throws Exception {
        
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable Exception ex) throws Exception {
    }
}
```

### 용도 예시
* 세부적인 보안 및 인증/인가 공통 작업
* API 호출에 대한 로깅 또는 감사
* `controller`로 넘겨주는 정보(데이터)의 가공

### `preHandle()`
* `controller`가 호출되기 전에 실행된다.
* 전처리 작업 또는 요청 정보를 가공해야할 때 사용할 수 있다.
* `preHandle`의 `handler` 인자 값은 `HandlerMapping`이 찾아준 컨트롤러 빈에 매핑되는 `HandlerMethod`라는 새로운 타입의 객체로써, `@RequestMapping`이 붙은 메소드의 정보를 추상화한 객체이다.
* 반환값이 `true`이면 다음 단계로 진행이 되지만, `false`라면 작업을 중단하여 이후의 작업(다음 인터셉터 또는 컨트롤러)은 진행되지 않는다.

### `postHandle()`
* `controller`가 호출된 후에 실행된다.
* 후처리 작업이 있을 때 사용할 수 있다.
* `controller`가 반환하는 `ModelAndView` 타입의 정보가 제공된다.
  * 최근 동향으로 RestAPI 기반의 컨트롤러(@RestController)를 만들면서 자주 사용되지는 않는다.
* `controller` 하위 계층에서 작업 중 예외가 발생하면 호출되지 않는다.

### `afterCompletion()`
* 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행된다.
* 요청 처리 중에 사용한 리소스를 반환할 때 사용하기에 적합하다.
* `controller` 하위 계층에서 작업 중 예외가 발생해도 호출된다.
