# ⛳️ JPA 엔티티 매핑 방법
### JPA 란?
* Java Persistence Api
* ORM 기술들을 자바에서 사용하기 위한 표준

## JPA 엔티티 매핑
1. 클래스는 public, protected로 선언돼있고, 매개변수가 없는 생성자를 반드시 가져야 함.
2. 클래스를 final로 지정하면 안됨.

```@Entity```
> 엔티티로 사용하려면 반드시 붙여야 하는 애노테이션  
> javax.persistence.Entity 애노테이션 이용  
> name 옵션이 있고 default는 클래스의 이름

```@Table```
> ddl 같은 테이블 세부사항을 조정하고 싶을 때 사용하는 애플리케이션  
> 주요 옵션은 name, schema, catalog, indexs 등 설정 가능  
> name 옵션은 기본 값은 Entity의 이름

```@Column```
> 컬럼 매핑 애노테이션  
> 기본적으로 필드에는 자동적으로 @Column이 붙음  
> 옵션으로 name, unique, nullable, columnDefinition 등이 있음  
> nullable의 기본값은 true, unique의 기본값은 false

```@Id```
> primary key 타입을 하는 필드에 이 애노테이션을 붙여야 함  
> primitive, wrapper, String, Date, BigInteger 타입이 가능

```@Enumerate```
> enum 타입을 매핑하고 싶을 때 사용  
> enum의 순서나 이름중 하나를 선택해서 DB에 저장가능

```@Temporal```
> 시간 관련 필드를 매핑하고 싶을 때 사용  
> BaseTimeEntity를 활용하여 시간 필드를 매핑하는 것이 좋다

```@Transient```
> 매핑하고 싶지 않은 멤버를 제외할 때 선언

```@OneToMany```
> 1에 해당하는 엔티티 클래스에서 Many에 해당하는 엔티티의 관계를 참조할 때 콜렉션으로 참조하고, 애노테이션을 @OneToMany를 붙여줌

```@ManyToOne```
> n에 해당하는 엔티티 클래스에서 1에 해당하는 엔티티의 관계를 참조할 때 1에 해당하는 엔티티의 객체타입을 참조하고, @ManyToOne 애노테이션을 붙임

**ManyToOne → 외래키 생성**  
**OneToMany → 조인테이블 생성**
