# JPA 엔티티 매핑

* 객체와 테이블 매핑: @Entity, @Table
* 필드와 컬럼 매핑: @Column
* 기본 키 매핑: @Id
* 연관관계 매핑: @ManyToOne,@JoinColumn


<br><br>


## 1. 객체와 테이블 매핑

### 1) @Entity
* @Entity가 붙은 클래스는 JPA가 관리(JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 필수적)
* 주의사항
  * 기본생성자가 반드시 필요하다. (parameter가 없는 public 혹은 protected 생성자)
  * final클래스, enum, interfacem inner 클래스는 사용할 수 없다.
  * 저장할 필드에 final 키워드를 사용할수 없다.

<br>

### 2) DB 스키마 자동 생성
* DDL을 애플리케이션 실행 시점에 자동으로 생성해준다
* 객체 중심으로 개발하기 용이하게 해준다
* DB방언을 활용해서 각 DB에 맞는 DDL을 생성해준다
* 이렇게 생성된 DDL은 local 개발 환경에서만 활용하고, 실 서비스 운영 환경에서는 사용하지 않거나, 생성된 DDL을 검토 후 사용한다.
#### (2-1) DB 스키마 자동 생성 - 속성
* create : 기존 테이블 삭제 후 재생성 (DROP + CREATE)
* create-drop : create와 같으나 종료 시점에 테이블 DROP
* update : 변경분만 반영(운영DB에는 사용하면 안된다)
* validate : Entity와 table이 정상 매핑되었는지만 확인
* none : 사용하지 않음
* 운영서버에는 사용하지 말고, 사용하게 되면 validate 나 none 만 선택적으로 사용해야한다.   
create, create-drop 같은 경우는 운영 DB 테이블과 함께 데이터가 소실될 우려가 있고,   
update 같은 경우는 규모에 따라 DB 가 rock 이 걸려 버려서 서비스가 잠시 중단되는 등의 위험성이 존재.


<br><br>


## 2. 필드와 컬럼 매핑

### 1) 매핑 어노테이션

* `@Column` : 컬럼매핑
* `@Temporarl` : 날짜 타입 매핑
* `@Enumerated` : enum 타입 매핑
* `@Lob` : BLOB, CLOB 매핑
* `@Transient` : 특정 필드를 컬럼에 매핑하지 않음 (매핑 무시)

#### (1-1) @Column
* `name` : 필드와 매핑할 테이플의 컬럼 이름
* `insertable, updatable` : 등록, 수정 가능 여부
* `nullable(DDL)` : null 허용여부 설정(**false**로 설정하면, DDL 생성시 not null 제약조건 붙음)
* `unique(DDL)` : @Table의 uniqueConstraints와 같지만, 한 컬럼에 한정하여 유니크 제약조건을 걸때 사용. 하지만 **unique** 를 사용하게 되면, 로그에서 정확히 어디에서 어떤 제약조건을 위배하여 에러가 났는지 파악하기 힘들게 랜덤한 메시지를 전달해주는 경우가 있기 때문에, 실무에서는 사용하지 않거나, **@Table의 uniqueConstraints** 를 활용한다.
* `columnDefinition(DDL)` : DB 컬럼 정보를 직접 설정 가능
* `length(DDL)` : 문자 길이 제약조건(String 타입에만 사용)
* `precision,scale(DDL)` : BigDecimal 타입에서 사용.(BigInteger도 사용 가능) precision은 소수점을 포함한 전체 자릿수, scale은 소수의 자릿수. double, float 타입에는 적용되지 않으며, 아주 큰 숫자나 정밀하게 소수를 다뤄야 할때 사용.

#### (1-2) @Enumerated
1. `EnumType.ORDINAL` : enum 순서를 DB에 저장 (기본값)
   * 실무에서는 **ORDINAL** 을 잘 쓰지 않는다. enum을 추가하거나 하게 되면 순서가 변경되어버리기 때문이다. -> 순서가 꼬여버린 상태로 DB에 데이터가 쌓이게 되면 추후에 해결하기 힘든 버그가 되어버림.
2. `EnumType.STRING` : enum 이름을 DB에 저장

#### (1-3) @Temporal
* java 8 이후 버전으로 `LocalDate, LocalDateTime` 을 쓰게 된다면 `@Temporal`은 쓸 필요가 없다.
* `TemporalType.DATE` : 날짜, 데이터베이스 date 타입과 매핑 (예: 2013–10–11)
* `TemporalType.TIME` : 시간, 데이터베이스 time 타입과 매핑 (예: 11:11:11)
* `TemporalType.TIMESTAMP` : 날짜와 시간, 데이터베이 스timestamp 타입과 매핑(예: 2013–10–11 11:11:11)

#### (1-4) @Lob
* 매핑하는 타입이 문자면 CLOB이 매핑되고, 나머지는 BLOB이 매핑된다.

#### (1-5) @Transient
* 필드 매핑 X
* DB에 저장, 조회 X


<br><br>


## 3. 기본 키 매핑

### 1) 기본 키 매핑 어노테이션
* @Id
* @GeneratedValue

<br>

### 2) 기본 키 매핑 방법
* 직접 할당 : `@Id`만 사용
* 자동 생성(`@GeneratedValue`)
  * `IDENTITY` : 데이터베이스에 위임. 주로 MYSQL,PostgreSQL, SQL Server, DB2에서 사용
    * 예) MySQL의 AUTO_ INCREMENT
    * JPA는 보통 트랜잭션 커밋 시점에 INSERT SQL 실행
    * AUTO_ INCREMENT는 데이터베이스에 INSERT SQL을 실행한 이후에 ID 값을 알 수 있음
    * IDENTITY 전략은 em.persist() 시점에 즉시 INSERT SQL 실행하고 DB에서 식별자를 조회
  * `SEQUENCE` : 데이터베이스 시퀀스 오브젝트 사용
    * 데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트 (예: 오라클 시퀀스)
    * 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용
    * @SequenceGenerator 필요
  * `TABLE` : 키 생성용 테이블 사용, 모든 DB에서 사용
    * @TableGenerator 필요
  * `AUTO` : 방언에 따라 자동 지정, 기본값


<br><br>


## 참고
[자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)