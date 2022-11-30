## HTTP
### HTTP(Hypertext Transfer Protocol)
* `HTTP`는 `W3`(World Wide Web)에서 정보를 주고 받을 수 있는 프로토콜이다.
* `HTTP`는 주로 TCP/IP 프로토콜을 사용한다.
* `HTTP`는 무상태 프로토콜이다.
  * 요청에 대해 상태를 저장하지 않는다.
  * 각각의 요청과 응답은 독립적이다.
* 클라이언트와 서버 사이에서 이루어지는 요청/응답 프로토콜이다.

### HTTP 요청 메세지
* HTTP 요청 메세지는 크게 세 부분으로 구성되어 있다.
* `Start Line`
  * Start Line 안에서도 세 부분으로 구성된다.
  * 예제) GET /user/search HTTP/1.1
  * Request Method
    * HTTP 1.0, HTTP 1.1
    * GET- 클라이언트가 서버에서 웹 리소스를 받아올 때 사용한다.
    * HEAD - HEAD를 사용하면 GET을 통해 받아오는 Response의 Header를 받아올 수 있다.
    * POST - 웹 서버에 데이터를 보낼 때 사용한다.   

    * HTTP 1.1
    * PUT - 서버에 데이터 저장을 요청할 때 사용된다
    * DELETE - 서버에 데이터 삭제를 요청할 때 사용된다.
    * TRACE - 수행 작업의 diagnostic trace를 요청할 때 사용된다.
    * OPTIONS - 지원하는 Request 목록을 받아올 때 사용한다.
    * CONNECT - 프록시에게 다른 호스트에 연결하고 콘텐츠를 분석하거나 캐시 없이 응답하도록 지시하는 데 사용된다.
  * Request URI
    * 요청하려는 리소스를 의미하며, URI(Uniform Resource Identifiers)를 이용하여 자원의 위치를 찾는다.
  * HTTP Version
    * HTTP/1.0, HTTP/1.1
* `Headers`
  * Host - 도메인 이름
  * Accept - MIME 타입으로 표현되는 클라이언트가 이해 가능한 컨텐츠 타입이 무엇인지 알려준다.
  * Accept-Language - 어떤 언어를 클라이언트가 이해할 수 있는지, 그리고 지역 설정 중 어떤 것이 더 선호되는지를 알려준다.
  * Accept-Encoding - 클라이언트가 이해 가능한 컨텐츠 인코딩이 무엇인지 알려준다.
  * Content-Type - 클라이언트에게 반환된 컨텐츠의 유형이 실제로 무엇인지 알려준다.
  * Content-Length - 수신자에게 보내지는 바이트 단위를 가지는 body의 크기를 나타낸다.
  * Connection - 요청이 끝난 후에 클라이언트와 서버가 계속해서 커넥션을 유지할 것인지를 제어한다. (Keep-Alive)
  * User-Agent - 요청을 보내는 클라이언트에 대한 정보를 나타낸다.
* `Body`
  * HTTP 요청 내용이다.
  * `Request Method`에 따라서 있을 수도 혹은 없을 수도 있다.
### HTTP 응답 메세지
* HTTP 응답 메세지도 세 부분으로 구성되어 있다.
* `Status Line`
  * 요청에 대한 응답 상태를 나타낸다.
  * Status Line에서도 세 부분으로 구성된다.
  * 예제) HTTP/1.1 404 Not Fount
  * HTTP Version
    * HTTP/1.0, HTTP/1.1
  * Status Code
    * 1XX, 2XX, 3XX, 4XX, 5XX
  * Status Text
    * 응답 상태에 대한 설명 부분
* `Headers`
  * HTTP 요청 메세지의 `Headers`와 동일하며, 일부 응답 메세지에만 추가된 헤더가 있다.
* `Body`
