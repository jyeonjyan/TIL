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