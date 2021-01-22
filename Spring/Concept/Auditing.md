# JPA Auditing으로 생성일/수정일 자동화하기
## 🙉 동기
> ### project에서 해당 data의 생성시간과 수정시간을 관리해야 할 부분이 있었다.  
> ### 주문 도메인에서 주문한 시간과 주문 내용을 수정한 시간이 필요했다.  
> ### 그리고 결제 도메인에는 결제 요청한 시간과 결제 내용을 수정하는 시간이 필요했다.

## JPA Auditing으로 생성시간/수정시간 자동화하기
보통 엔티티는 해당 데이터의 생성시간과 수정시간을 포함한다.  
이렇다 보니 매번 DB에 insert하기 전, update하기 전에 날짜 데이터를 등록/수정하는 코드가 여기저기 들어간다.
```java
public void savePosts() {
  ...
  posts.setCreateDate(new LocalDate());
  postsRepository.save(posts);
  ...
}
```
이런식으로 단순하고 반복적인 코드가 모든 테이블과 서비스 메서드에 포함되어야 한다고 생각하면 어마어마 하게 귀찮고 코드가 지저분해진다. 이 문제를 해결할 수 있는 것이 JPA Auditing.

## 적용하기
```java
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;

    public LocalDateTime getCreatedDate() {
        return createdDate;
    }

    public LocalDateTime getModifiedDate() {
        return modifiedDate;
    }
}
```
* ```@MappedSuperclass```: JPA Entity 클래스들이 BaseTimeEntity를 상속할 경우 필드들도 칼럼으로 인식하도록 해줌.
* ```@EntityListeners(AuditingEntityListener.class)```: BaseTimeEntity 클래스에 Auditing 기능을 포함시킨다.
* ```@CreatedDate```: Entity가 생성되어 저장될 때 시간이 자동 저장됩니다.
* ```@LastModifiedDate```: 조회한 Entity의 값을 변경할 때 시간이 자동으로 저장.

**JPA Auditing 어노테이션을 모두 활성화, Application 클래스에 활성화 어노테이션 추가**
```java
@EnableJpaAuditing
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(CaffeineApplication.class, args);
    }
}
```