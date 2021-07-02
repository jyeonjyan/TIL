# 트랜잭션(Transaction) 이란?

## 개요
트랜잭션은 다양한 곳에서 의미를 가지고 있다.  
데이터베이스에서는 상태를 변화시키기 위해 수행하는 작업의 단위를 뜻한다.  
일반적인 컴퓨터 과학에서는 쪼갤 수 없는 업무처리의 단위를 의미하기도 한다.  

## 트랜잭션 성질
* 원자성: 한 트랜재션 내에서 실행한 작업은 하나로 간주한다. 즉 모두 성공 혹은 실패
* 일관성: 트랜잭션은 데이터 인터그리티 만족 등 일관성있는 데이터베이스 상태를 유지한다.
* 격리성: 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않게 격리한다.
* 지속성: 트랜잭션이 성공적으로 실행되면 결과는 항상 저장된다.

## 개발에서의 트랜잭션
DB 접근이 발생하는 여러 단위 작업들을 의미있는 그룹으로 묶어서 일괄 커밋 또는 일괄 롤백하는 매커니즘을 뜻한다.

1. 프레임워크들에서는 다음과 같은 기법들을 사용하거나 지원한다.
2. 선언적 트랜잭션 관리 방법 사용
   1. 어노테이션 방식으로 `@Transactional`을 선언하여 사용하는 방법이 일반적이며, 이를 선언적 트랜잭션이라고 부른다
3. JDBC
   1. JDBC, MyBatis 등을 사용할 때는 `DataSourceTransactionManager`를 관리자로 등록한다.
4. JPA 트랜잭션 방식 지원
   1. JPA 트랜잭션은 `JpaTransactionManager`를 Bean으로 등록하여 사용한다.
   2. 이 클래스는 프로퍼티를 통해 전달받은 `EntityManagerFactort`를 이용하여 트랜잭션.
5. JTA 트랜잭션 방식 지원
   1. 다중 자원에 접근 시 `Java Transaction API` 를 사용하는데, JTA 트랜잭션은 `JtaTransactionManager`를 Bean으로 등록하여 사용한다.
6. Nested(중첩) Transation 지원
7. Lazy Connection
   1. Lazy Connection이란 쿼리 호출 시점에 해당 데이터소스만 연결하는 것이다.
   2. Lazy Connection적용 시, 여러 Lazy Bean Id를 리스트로 등록하고 미적용 시 일반적인 데이터소스 BeanId를 리스트로 등록한다.
8. 타임아웃 속성 지원
   1. 지정한 시간 내에 해당 메소드 수행이 완료되지 않으면 롤백 수행. -1이라면 타임아웃이 없다.
   2. `@Transactional(timeout=10)`

## 선언적 트랜잭션
선언적 트랜잭션 방법은 태그를 사용하는 방법과 `@Transactional` 어노테이션을 사용하는 방법이 있다. 

## 트랜잭션 속성(Propagation - Nested Transaction)
> 한 트랜잭션 내부에서 중첩된 트랜잭션 처리가 필요할 때 사용하는 기능이다. Spring은 propagation 속성을 통해 Nested Transaction을 지정한다.

* REQUIRED: 이미 시작된 트랜잭션이 있으면 참여하고 없으면 새로 시작한다.
* SUPPORTS: 이미 시작된 트랜잭션이 있으면 참여하고 없으면 트랜잭션 없이 진행.
* REQUIRED_NEW: 부모 트랜잭션을 무시하고 항상 새로운 트랜잭션을 실행한다.
* NESTED: 메인 트랜잭션 내부에 중첩된 트랜잭션을 시작한다.
  
## 6. Transactional 속성 (Isolation)
* DEFAULT: 기본 설정, 기본 격리 수준
* SERIALIZABLE : 가장 높은 격리, 성능 저하 가능성 있음
* READ_UNCOMMITED : 커밋되지 않은 데이터 읽을 수 있음
* READ_COMMITED : 커밋된 데이터에 대해 읽기 허용
* REPEATABLE_READ : 동일 필드에 대한 다중 접근 시 동일 결과 보장