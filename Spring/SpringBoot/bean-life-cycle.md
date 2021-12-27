# 빈 생명주기와 콜백

## 들어가며..
spring 애플리케이션에는 데이터베이스 커넥션 풀이나, 네트워크 애플리케이션 시작 시점에 필요한 연결을 미리 해두고  
애플리케이션 종료 시점에 연결을 모두 종료하는 작업을 진행하려면 객체의 초기화와 종료 작업이 필요하다.

## 코드로 알아보기 
* `DatabaseClient`는 `connect()`를 통해 연결을 맺고, `disconnect()`를 통해 연결을 끊어야 한다.

DatabaseClient.java
```java
public class DatabaseClient {
    private String url;

    public void setUrl(String url) {
        this.url = url;
    }

    public DatabaseClient(){
        System.out.println("===== 생성자 호출 url: "+url);
        connect();
        call("초기화 연결 메시지");

    }

    public void connect(){
        System.out.println("======= connect: "+url);
    }

    public void call(String message){
        System.out.println("call = " + url + " message: " + message);
    }

    public void disconnect(){
        System.out.println("====== close: "+url);
    }
}
```

BeanLifeCycleTest.java
```java
public class BeanLifeCycleTest {
    @Test
    @DisplayName("빈 생명주기 콜백")
    void beanLifeCycle(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        DatabaseClient bean = ac.getBean(DatabaseClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig {
        @Bean
        public DatabaseClient databaseClient(){
            DatabaseClient databaseClient = new DatabaseClient(); // 이 시점에서 객체가 생성 됨.
            databaseClient.setUrl("https://jyeonjyan.me/database");
            return databaseClient;
        }
    }
}
```

### 결과 예측하기

`beanLifeCycle()` 테스트를 실행시키면..  
뭔가 `LifeCycleConfig`로 컨테이너를 구성하기에 `DatabaseClient` 가 생성되고 빈으로 등록되는 시점에 .. `setUrl()` 이 있기 때문에 url 이 등록되고 생성되어

```sh
===== 생성자 호출 url: https://jyeonjyan.me/database
======= connect: https://jyeonjyan.me/database
call = https://jyeonjyan.me/database message: 초기화 연결 메시지
```

위와 같이 출력될 것이라 충분히 예상할 수 있지만..   
그럼 당신은 아직 스프링 빈의 라이프사이클을 정확히 이해하지 못한 것이다.

스프링 빈은 간단하게 다음과 같은 라이프사이클을 가진다.  
**객체 생성 -> 의존관계 주입**

스프링 빈은 객체를 생성하고, 의존관계 주입이 다 끝난 다음에야 필요한 데이터를 사용할 수 있는 준비가 완료된다.  
따라서 초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출해야 한다.  
그런데 개발자가 의존관계 주입이 모두 완료된 시점을 어떻게 알 수 있을까?

스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해서 초기화 시점을 알려주는 다양한 기능을 제공한다.  
또한 스프링은 스프링 컨테이너가 종료되기 직전에 **소멸 콜백**을 준다.

스프링 빈의 이벤트 라이프사이클
**스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸전 콜백 -> 스프링 종료**