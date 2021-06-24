## JPQL
JPA 를 사용하면 객체를 중심으로 개발  
JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공  
JPQL은 엔티티 객체를 대상으로 쿼리, SQL은 데이터베이스 테이블을 대상으로 쿼리  

```java
String jpql = "select m From Member m where m.name like '%hello%'";
List<Member> result = em.createQuery(jpql, Member.class)
                        .getResultList();
```

### JPQL 문법
```java
select m from Member m where m.age > 18
```
* 엔티티와 속성은 대소문자 구분(Member, username)
* JPQL 키워드는 대소문자 구분 안함(SELECT, FROM, WHERE)
* 엔티티 이름을 사용, 테이블 이름이 아님(Member)
* 별칭은 필수(m)

### Named 쿼리 - 정적 쿼리
* 미리 정의해서 이름을 부여해두고 사용하는 JPQL
* 어노테이션, XML에 정의
* 애플리케이션 로딩 시점에 초기화 후 재사용
* 애플리케이션 로딩 시점에 쿼리 검증
* query.getResultList() - 결과가 하나 이상, 리스트 반환
* query.getSingleResult() - 결과가 정확히 하나, 단일 객체 반환(아니면 Exception)

```java
@Entity
@NamedQuery(
    name = "Member.findByUserName",
    query = "select m from m where m.username =: username")
)
public class Member{
}
List<Member> resultList = 
    em.createQuery("Member.findByUserName", member.class)
    .getResultList();
```