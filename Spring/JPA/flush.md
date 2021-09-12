## 플러시란?
> 영속성 컨텍스트를 정리하면서 문득 궁금증이 생겼다.  
> commit 하고 flush에 차이가 뭘까? 이런 궁금증이다.

### 플러시
* 영속성 컨텍스트의 변경 내용을 DB에 반영하는 것을 말한다.
```java
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();
//영속상태
em.persist(account);

Account getAccount = em.find(Account.class, 1);
getAccount.setEmail("change@mail.com");
em.getTransaction().commit();// transaction 커밋
```

* Transaction commit 이 일어날 때 flush가 동작하는데, 이때 쓰기 지연 저장소에 쌓아 놨던 insert, update, delete SQL들이 DB에 날라간다.
  * 주의 ! 그렇다고 영속성 컨텍스트를 비우는 것이 아니다.
* 쉽게 얘기하자면 영속성 컨텍스트의 변경 사항들과 DB의 상태를 맞추는 작업이다.
  * 플러시는 영속성 컨텍스트의 변경 내용을 DB에 동기화 한다.

### 플러시의 동작 과정
1. 변경을 감지한다(Dirty Checking)
2. 수정된 Entity를 쓰기 지연 SQL 저장소에 등록한다.
3. 쓰기 지연 SQL 저장소의 Query를 DB에 전송한다. (등록, 수정, 삭제 Query)

* flush 가 발생했다고 해서 commit 이 이루어지는것이 아니고, flush 다음에 실제 commit 이 일어난다.
* 플러시가 동작할 수 있는 이유는 데이터베이스 트랜잭션이라는 개념이 있기 때문이다
  * 트랜잭션이 시작되고 해당 트랜잭션이 **commit 되는 시점 직전에만 동기화 해주면 되기 때문에**, 그 사이에서 플러시 메커니즘 동작이 가능한 것이다.
* JPA는 기본적으로 **데이터를 맞추거나 동시성에 관련된 것들은 데이터베이스 트랜잭션**에 위임한다.

### 영속성 컨텍스트를 플러시하는 방법
1. em.flush() 를 통한 직접 호출

```java
// 영속 상태 (Persistence Context 에 의해 Entity 가 관리되는 상태)
Member member = new Member(200L, "A");
entityManager.persist(member);

entityManager.flush(); // 강제 호출 (쿼리가 DB 에 반영됨)

System.out.println("DB INSERT Query 가 즉시 나감. -- flush() 호출 후 --  Transaction commit 됨.");
tx.commit(); // DB에 insert query 가 날라가는 시점 (Transaction commit)
```

**플러시가 일어나면 1차캐시는 그대로 남아있고 Query들만 DB에 전송된다.**

2. 트랜잭션 커밋 시 플러시 자동 호출
3. JPQL 쿼리 실행 시 플러시 자동 호출

```java
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);

// 중간에 JPQL 실행
query = entityManager.createQuery("select m from Member m", Member.class);
List<Member> members = query.getResultList();
```

JPQL 의 기본 모드는 `flush()` 를 자동으로 날리기 때문에 select JPQL 쿼리 실행 시 조회가 가능하다.

### 플러시 모드 옵션

* em.setFlushMode(FlushModeType.COMMIT);
* FlushModeType.AUTO
* 기본 설정 값
* Transaction 을 commit 하거나 Query를 실행할 때 flush()를 먼저 수행한다.
* FlushModeType.COMMIT
* Transaction commit 할때만 flush()를 먼저 수행한다.
* Query를 실행할 때는 flush()를 먼저 수행하지 않는다.
* 이점: persist 한 것과 전혀 다른 테이블을 조회하는 경우