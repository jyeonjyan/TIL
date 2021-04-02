# 양방향 연관관계와 연관관계의 주인

## 연관관계 누구를 주인으로?
> 외례키가 있는 곳으로 주인으로 정해라
```java
public class Team {
    @OneToMany
    private List<Member> members = new ArrayList<Member>();
}
```
```java
public class Member {
    @ManyToOne(mappedBy="team")
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

## 이유는?
그냥 이상하다!! ㅋㅋㅋㅋㅋㅋ  
JPA를 엄청 잘하면 One으로도 이해할 수 있지만 일반적으로는 Many인 부분에 mappedBy 한다.