## JPA(Java Persistence API)
### 개념
* 자바 진영에서의 ORM(Object-Realational Mapping) 기술 표준으로 사용되는 인터페이스 모음이다.
  * ORM이란 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑 해주는 것을 의미한다.
* 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크이다.
* JPA를 구현한 대표적인 오픈소스로는 Hibernate가 있다.

### 사용 이유
* SQL 중심 개발에서 객체 중심 개발이 가능하다.
* 생산성이 증가한다.
  * DDL 자동 생성
  * 간단한 메서드로 CRUD 가능
  * SQL을 작성하지 않아도 된다.
* 유지보수가 쉽다.
  * 일반 쿼리의 경우 필드가 변경되면 기존에 만들어진 쿼리를 모두 변경해야한다.
  * JPA는 필드만 변경 혹은 추가해주면 된다.
* RDBMS와 객체간의 패러다임 불일치 해결
  * 상속 / 연관관계 / 객체 그래프 탐색 / 비교하기 같은 패러다임 불일치를 해결한다.
  * 객체 상속 관계는 테이블 수퍼타입 서브타입 모델링으로 해결한다.
  

### 동작 구조
* 애플리케이션과 JDBC 사이에서 동작한다.
* JPA 내부에서 JDBC API를 사용하여 SQL을 호출해서 DB와 통신한다.

### 용어
* `JPQL`
  * 테이블이 아닌 객체를 대상으로 검색하는 객체 지향 쿼리
  * SQL을 추상화해서 특정 데이터베이스 SQL에 의존 X
  * JPQL을 한 마디로 정의하면 객체 지향 SQL
* `EntityManagerFactory`
  * 애플리케이션에 생성 후 전체에서 공유
* `EntityManager`
  * 고객 요청당 1개씩 생성, 쓰레드간 공유 X
* `EntityTransaction`
  * JPA의 모든 데이터 변경은 트랜잭션 안에서 실행
* 준영속 엔티티
  * 영속성 컨텍스트가 더는 관리하지 않는 엔티티를 말한다.
  * 수정 방법
    * 변경 감지 사용(*): 변경된 값만 업데이트
    * 병합 사용: 준영속 상태의 엔티티를 영속 상태의 엔티티로 변경하는 방식, 모든 필드가 변경되어서 `null` 값이 들어갈 수 있다.

### 어노테이션
* `@Id`
  * 설정 프로퍼티가 테이블의 기본 키(Pk) 역할
	* getter에 설정 가능
* `@GeneratedValue`
	* 기본 키의 값을 자동으로 생성
	* 속성
	  * generator: SequenceGenerator, TableGenerator에 명시된 기본 키 생성자를 재사용할 때 사용.
	  * strategy : 기본 키 생성 전략으로 기본 값은 AUTO 이고, IDENTITY, SEQUENCE, TABLE 이 있다.
	* 예제
	  * @Id
	  * @GeneratedValue(strategy=GenerationType.AUTO)
* `@Embedded`, `@Embeddable`
	* 여러 데이터를 하나의 의미를 갖는 객체로 표현
	* JPA Entity안의 Column을 하나의 객체로 사용
	* 내장된 Column 객체에 @Embedded 표기
	* 해당 객체를 생성 후 @Embeddable 표기
* `@Inheritance`, `@DiscriminatorValue`
	* 상속관계 매핑
	  * 객체는 상속 관계가 존재, 관계형 데이터베이스는 상속 관계 X
	  * 슈퍼 타입 서브 타입 관계(모델링 기법)은 객체 상속과 유사
	  * 객체의 상속 구조와 DB의 슈퍼타입 서브타입 관계 매핑
	* 물리 모델 구현법
	  * 객체는 상속을 지원해서 모델링과 구현이 같다.
	  * DB는 상속을 지원하지 않아서 물리 모델로 구현할 방법이 필요하다.   
		* @Inheritance(strategy=InheritanceType.XXX)   
		  * XXX: JOINED(정규화), SINGLE_TABLE(단일 테이블), TABLE_PER_CLASS   
		* DiscriminatorColumn(name = "dtype"): 부모 클래스에 선언, 하위 클래스를 구분하려는 용도이며 기본 값 dtype이다.   
		* @DiscriminatorValue("XXX"): 하위 클래스에 선언, 엔티티를 저장할 때 슈퍼 타입의 구분 컬럼에 저장할 값을 지정한다.   
* `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`
  * 방향
	* 단방향 관계: 두 엔티티가 관계를 맺을 떄, 한 쪽의 엔티티만 참조하는 것을 의미
	* 양방향 관계: 두 엔티티가 관계를 맺을 때, 서로 참조하는 것을 의미
  * 다중성
	* 1:1, 1:N, N:1, N:M
	* 연관 관계의 주인(Owner)
	  * 두 테이블에 대한 외래키를 갖는 테이블이 연관 관계의 주인
	  * 연관 관계 주인만이 외래 키 관리(등록, 수정, 삭제)가 가능하고, 주인이 아닌 엔티티는 읽기만 가능
	* 속성
	  * fetch: @ManyToOne(fetch = Fectch.XXX)   
		* XXX: EAGER(즉시), LAZY(지연)   
		* XToOne 기본 값 -> EAGER, XToMany 기본 값 -> Lazy   
	  * cascade: @ManyToOne(fetch = Fectch.LAZY, cascade = CascadeType.XXX)   
		* XXX: ALL, PERSIST, MERGE, REMOVE, REFRESH, DETACH
		  1. ALL: 상위 엔티티에서 하위 엔티티로 모든 작업을 전파
		  2. PERSIST: 하위 엔티티까지 영속성 전달 - Order Entity 저장 시, OrderItem Entity 도 저장
		  3. MERGE: 하위 엔티티까지 병합 작업을 지속 - Order Entity, Person Entity 조회 후 업데이트
		  4. REMOVE: 하위 엔티티까지 제거 작업을 지속 - 연결된 하위 엔티티까지 엔티티 제거
		  5. REFRESH: DB 로부터 인스턴스 값 다시 읽어오기, 연결된 하위 엔티티 인스턴스도 새로고침
		  6. DETACH: 영속성 컨텍스트에서 엔티티 제거, 연결된 하위 엔티티까지 영속성 제거
		* 다중 연관 관계의 Entity 인 경우, 주의해서 사용
* `@JoinColumn`
  * 외래 키를 매핑할 때 사용: @JoinColumn(name = "item_id")
* `@Enumerated(EnumType.XXX)`
  * default ORDINAL, 숫자: 중간에 새로운 유형 추가가 되면 문제가 생김 -> 반드시 STRING으로 사용
* `@PersistanceContext`
  * EntityManager를 빈으로 주입할 때 사용
  * 스프링 부트에서는 @Autowired 로 주입 가능. (스프링도 차후 지원 예정)
  * 싱글톤 동시성 이슈 방지
	* 스프링 컨테이너가 초기화되면서 @PersistenceContext로 주입 받은 EntityManager를 Proxy로 감싼다.
	* EntityManager 호출 마다 Proxy를 통해 EntityManager를 생성하여 Thread-safe를 보장한다.
* `@RequiredArgsConstructor`
  * final이나 @NonNull인 필드 값만 파라미터로 받는 생성자 만듦
* `@NoArgsConstructor`
  * 파라미터가 없는 기본 생성자를 생성
* `@AllArgsConstructor`
  * 모든 필드 값을 파라미터로 받는 생성자를 만듦
