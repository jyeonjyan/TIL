# JPA 쿼리 성능 개선 사례

## 1. 연관된 Entity의 save를 위해서는 반대편 Entity ID만 있으면 된다.

<img src="../../img/query-per-entity-diagram.png" width="500px">

1번 예제는 위와 같은 엔티티를 이용해 차이를 확인해 볼게요!  
주문(OrderEntity)에서 매번 회원을 조회할 필요가 없을 때, 주문은 회원의 모든 정보(MemberEntity)를 알 필요가 없습니다. 주문은 `회원Id` 만을 갖고 있으면 됩니다.

### `회원Id`만을 멤버로 갖는 생성자 만들기

```java
public Member(String id) {
    this.id = id;
}
```

### 주문을 insert 할 때 `회원Id` 만을 연관관계로 갖고 있으면 됩니다.

```java
Order.builder()
        .id(null)
        .book(bookEntity)
        .member(new Member(memberEntity.getId()))
        .build();
```
