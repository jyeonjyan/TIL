## 다양한 JPA findBy 메서드

오늘은 하루종일 top10 을 어떻게 처리할까에 대한 고민을 했다  
처음에는 ```@Querydsl```로 리밋을 걸까 했지만 일반 JPA 메서드에서도 가능했었다.  

TableRepository.java
```java
List<TableDomain> findTop10ByOrderByGoodsDesc();
```

<br>

이렇게 하면 select query limit 이 10개 까지 걸림과 동시에 좋아요 갯수대로 정렬까지 되는걸 알 수 있다.

console
```shell
Hibernate: 
    select
        tabledomai0_.board_idx as board_id1_3_,
        tabledomai0_.content as content2_3_,
        tabledomai0_.goods as goods3_3_ 
    from
        board tabledomai0_ 
    order by
        tabledomai0_.goods desc limit ?

```