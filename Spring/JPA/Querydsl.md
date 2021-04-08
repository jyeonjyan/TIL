## QueryDsl이란?
> JPQL의 빌더 클래스

## QueryDsl 사용전 설정
dependency 추가
```
dependencies {
  compile("com.querydsl:querydsl-core:4.2.1")
  compile("com.querydsl:querydsl-apt:4.2.1")
  compile("com.querydsl:querydsl-jpa:4.2.1")
  compile("com.querydsl:querydsl-collections:4.2.1")
  ...
}
```

## QueryDsl 사용법
* example entity(다음 두 entity는 1:N 관계)
  * User
  * Account

* User.java
```java
@Entity
@Table(name = "tb_user")
@Getter @Setter
@EqualsAndHashCode
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @JsonIgnore
    private Long id;

    @Column(name = "name", nullable = false)
    private String name;

    @Column(name = "age", nullable = false)
    private Long age;

}
```

* Account.java
```java
@Entity
@Table(name = "tb_account")
@Getter @Setter
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @JsonIgnore
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "user_id", referencedColumnName = "id")
    @JsonIgnore
    private User userId;

    @Column(name = "account")
    private String account_num;

    @Column(name = "money")
    private Double money;

    @Column(name = "bank_name")
    private String bankName;

    @PrePersist
    public void initMoney() {
        this.money = 0D;
    }

}
```

* UserRepositoryImpl <QueryDsl 부분>

```java
 @Override
    public List<AccountUserJoinDto> findAllUserGreaterThanAge(Long age) {
        QUser user = QUser.user;
        QAccount account = QAccount.account;

        JPQLQuery<AccountUserJoinDto> query = from(account)
                .innerJoin(account.userId, user)
                .select(Projections.constructor(AccountUserJoinDto.class,
                        user.name,
                        user.age,
                        account.money,
                        account.bankName))
                .where(user.age.gt(age));

        return query.fetch();
    }
```

findAllUserGreaterThanAge 함수 호출시 다음과 같은 쿼리가 생성되어 DB에 전달됨. 다음 innerJoin함수를 보면 감이 오겠지만 account 변수의 위치에 주의하자.