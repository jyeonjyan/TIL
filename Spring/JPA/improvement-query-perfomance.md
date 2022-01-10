# JPA 쿼리 성능 개선 사례

## 연관관계의 주인은 엔티티를 직접적으로 갖고 있을 필요가 없다.

<img src="../../img/query-per-entity-diagram.png" width="500px">

우리 서비스는 위와 같은 엔티티, 연관관계를 가집니다.  
주문(OrderEntity)에서 매번 회원을 조회할 필요가 없을 때, 주문은 회원의 모든 정보(MemberEntity)를 알 필요가 없습니다.  

