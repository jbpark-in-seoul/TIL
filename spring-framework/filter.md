## 필터(Filter)
### 필터란?
* 필터는 `J2EE 표준 스펙` 기능으로 `디스패처 서블릿에 요청이 전달되기 전, 후`에 url 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공한다. 
  * 디스패처 서블릿은 스프링의 가장 앞단에 존재하는 프론트 컨트롤러이므로, `필터는 스프링 범위 밖`에서 처리가 되는 것이다.
* 스프링 컨테이너가 아닌 `웹 컨테이너에 의해 관리`된다.

```java
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    public default void destroy() {}
}
```

### 용도 예시
* 공통된 보안 및 인증/인가 관련 작업(XSS 방어)
* 모든 요청에 대한 로깅 또는 감사
* 이미지/데이터 압축 및 문자열 인코딩
* 스프링과 분리되어야 하는 기능

### `init()`
* `init` 메서드는 필터 객체를 초기화하고 서비스에 추가하기 위한 메서드이다.
* 웹 컨테이너가 요청 전 필터 객체를 초기화한 후 `doFilter()` 메서드를 통해 요청을 처리한다.

### `doFilter()`
* `doFilter` 메서드는 url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메서드이다.
* 필터는 여러 개 정의할 수 있다.
  * `FilterChain` 인자 값을 이용해 여러 개의 필터를 연결 시킬 수 있다.
  * `chain.doFilter(request, response);`
* 실행 순서
  * `init` - `doFilter` - `controller` - `doFilter` - `destroy`

```java
// 첫 번째 필터
@Slf4j
public class FirstFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("init first filter");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("first doFilter");
        filterChain.doFilter(servletRequest, servletResponse);
        log.info("first doFilter close");
    }

    @Override
    public void destroy() {
        log.info("destroy first filter");
    }
}
```

```java
// 두 번째 필터
@Slf4j
public class SecondFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("init second filter");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("second doFilter");
        filterChain.doFilter(servletRequest, servletResponse);
        log.info("second doFilter close");
    }

    @Override
    public void destroy() {
        log.info("destroy second filter");
    }
}
```

```java
// 필터 빈 설정
@Configuration
public class Config{

    @Bean
    public FilterRegistrationBean firstFilterRegistrationBean() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean(new FirstFilter());
        registrationBean.setOrder(1);
        return registrationBean;
    }

    @Bean
    public FilterRegistrationBean secondFilterRegistrationBean() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean(new SecondFilter());
        registrationBean.setOrder(2);
        return registrationBean;
    }
}
```
