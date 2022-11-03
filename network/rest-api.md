## REST API(Representational State Transfer API)
* `REST API`란, REST 아키텍처의 제약 조건을 준수하는 애플리케이션 프로그래밍 인터페이스를 말한다.
* `REST`란 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미합니다.
  * `HTTP URI`를 통해 자원을 명시한다.
  * `HTTP Method`(Post, Get, Put, Delete, Patch)를 통해 자원을 CRUD 처리한다.

### REST의 특징
1. Server-Client(서버-클라이언트 구조)
   * 클라이언트와 서버의 역할이 구분되어 있다.
2. Stateless(무상태)
   * 작업에 대한 상태를 기억할 필요 없이 들어온 요청에 대해 처리만 해주면 된다.
3. Cacheable(캐시 처리 가능)
   * HTTP 웹 표준을 사용하여 캐시를 사용할 수 있다.
   * 응답 시간이 빠르다.
4. Self-descriptiveness)
   * `JSON`, `XML` 등을 이용하여 메세지를 직관적으로 이해할 수 있고, 요청의 행위를 알 수 있다.
5. Layered System(계층화)
   * 클라이언트와 서버가 분리되어 있어서 중간에 프록시 서버, 암호화 계층 등을 중간 매체로 사용할 수 있다.
6. Uniform Interface(인터페이스 일관성)
   * HTTP 표준에만 따른다면 모든 플랫폼에서 사용이 가능하다.
   * 특정 언어나 기술에 종속되지 않는다.

### REST 구성 요소
1. 자원(Resource): HTTP URI
2. 자원에 대한 행위(Verb): HTTP Method
3. 자원에 대한 행위의 내용(Representations): HTTP Message Pay Load

### REST의 장단점
* 장점
  * 가독성: REST API 메세지를 읽는 것 만으로도 메세지가 의도하는 바를 명확하게 파악할 수 있다.
  * 서버-클라이언트 구조: HTTP 프로토콜 서비스를 충족하면 다양한 플랫폼에서 원하는 서비스를 개발 및 배포할 수 있다.
  * `JSON`, `XML` 등 다양한 언어를 이용하여 작성할 수 있고, 간결한 헤더 표현을 통해 가독성을 향상 시킬 수 있다.
* 단점
  * 공식화된 API 표준이 없다.
  * 메소드의 형태가 제한적이다.

### 사용 규칙
1. 슬래시 구분자는 계층 관계를 나타낼 때 사용된다.
2. 하이픈은 가독성을 높이는데 사용한다.
3. `URI`의 마지막에는 `슬래시`를 포함하지 않는다.
4. `URI`에는 `밑줄`을 사용하지 않는다.
5. `URI`에는 `소문자`를 사용한다.
6. `URI`에는 `파일 확장자`를 포함하지 않는다.
   * `Accept Header`를 사용한다
   * ex) GET: `http://restapi.exam.com/orders/2/Accept: image/jpg`

### `RESTful API` 개발 원칙
1. 자원을 식별할 수 있어야 한다.
   * `URL`만으로 내가 어떤 자원을 제어하려고 하는지 알 수 있어야 한다.      
     자원을 제어하기 위해서 자원의 위치, 자원의 종류를 알 수 있어야 한다.
2. 행위는 명시적이야 한다.
     * `HTTP Method`의 사용처에 맞게 사용해야 한다.
       * `POST`: create
       * `GET`: read
       * `PUT`: update
       * `DELETE`: delete
3. 자기 서술적이어야 한다.
     * 데이터에 대한 메타 정보만으로도 어떤 종류의 데이터인지 알 수 있어야 한다.
     * 데이터 처리를 위해 원본 데이터를 읽어야 한다면 자기 서술적이지 못 하다.
