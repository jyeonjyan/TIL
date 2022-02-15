# Spring persistence context 그리고 transactional
> 기선님 [블로그(트랜잭션과 스프링 AOP)](https://www.whiteship.me/spring-transactional-and-spring-aop/)를 보다가 실습하게 된 내용을 공유한다.

## 기선님 예제의 시나리오는 아래와 같다.
1. JpaRunner 라는 클래스에 `savePost()`라는 메소드가 존재한다.
2. `savePost()`는 Post 객체(sp)를 생성하고, `insert` 하는 동시에 `FindById()` JPA 메소드를 사용해서 객체(sp)를 다시 `select` 한다.
3. `entityManger.contains(sp)` 는 true를 반환할 것이다.

```java
@Component
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext
    EntityManager entityManager;

    @Autowired
    PostRepository postRepository;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        savePost();
    }

    @Transactional
    private void savePost() {
        Post post = new Post();
        post.setTitle("keesun");

        Post newPost = postRepository.save(post);
        System.out.println(postRepository.findById(newPost.getId()));
        System.out.println(entityManager.contains(newPost)); // expected true
    }
}
```

하지만 결과는 `false`.  
결론은 `savePost()` 메소드에 트랜잭션이 적용되지 않았다.  
트랜잭션이 적용되지 않은 이유는 `@Transactional` 이해하고 있다면 쉽게 알아낼 수 있다고 한다.

## @Transactional은 AOP 기반이며, AOP는 다이나믹 프록시를 기반으로 동작한다.

Spring 3대 핵심 원리 중 하나인 AOP 기술은 내부적으로 다이나믹 프록시를 기반으로 동작한다.  
프록시 기반 AOP의 단점 중에 하나인 프록시 내부에서 내부를 호출할 때는 부가적인 서비스(예제에서는 `@Transactional`)가 적용되지 않는다.  

호출하려는 타겟을 감싸고 있는 프록시를 통해야만 부가적인 기능이 적용되는데 프록시 내부에서 **내부를 호출 할 떄는 감싸고 있는 영역을 거치지 않기 때문이다.**

### 그림을 참고하면 아래와 같다.

<p align="center">
    <img src="../../img/transactional-based-aop.png" width="700px">
</p>

> 프록시로 감싼 타겟 (JpaRunner)를 외부에서 호출할 때 run()이라는 public 메소드를 호출하는데 (ApplicationRunner를 구현했기 때문에) 이 때 run() 메소드에는 트랜잭션이 적용되지 않는다.  
> 왜냐면 @Transactional을 savePost()만 붙였으니까. 그런데 그렇게 호출한 run()이 내부에서 @Transactional을 사용한 savePost()를 호출하더라도, JpaRunner 밖에서 호출이 되는게 아니라 프록시 내부에서 savePost()를 바로 호출하기 때문에 타겟을 감싼 트랜잭션이 적용되지 않는 것이다.

**차라리 JpaRunner 밖에서 savePost() 메소드를 바로 호출했다면 트랜잭션이 적용됐을 것이다.**

ex. `JpaExController.java / public void createPost(jpaRunner.savePost()){}`

## 따라서 실습하기