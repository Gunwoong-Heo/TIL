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
#### (2-1). DB 스키마 자동 생성 - 속성
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

> **hibernate.hbm2ddl.auto**

* `@Column` : 컬럼매핑
* `@Temporarl` : 날짜 타입 매핑
* `@Enumerated` : enum 타입 매핑
* `@Lob` : BLOB, CLOB 매핑
* `@Transient` : 특정 필드를 컬럼에 매핑하지 않음 (매핑 무시)











<br><br>


## 참고
[자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)