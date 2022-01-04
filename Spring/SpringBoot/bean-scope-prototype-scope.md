# 빈 스코프, 프로토타입 스코프
> 스코프는 빈이 존재할 수 있는 범위를 말한다.

* 싱글톤: 기본 스코프, 스프링 컨테이너 시작과 종료까지 유지되는 가장 넓은 범위의 스코프.
* 프로토타입: 스프링 컨테이너는 빈 생성과 의존관게 주입, 초기화까지만 관여하고 더는 관여하지 않는 가장 짧은 범위의 스코프. (종료 메소드 호출이 안됨)
* 웹 관련 스코프
  * request: 웹 요청이 들어오고 나갈때 까지 유지되는 스코프.
  * session: 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프.
  * application: 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프.

## 싱글톤 스코프
> 기본으로 만들면 싱글톤 스코프이다. (코드로 확인해 보자)

싱글톤 스코프, 빈으로 등록하여 공유할 자원(클래스)을/를 만든다.
```java
public class SingletonScopeEx {
    private int singletonInteger = 0;

    public int addInteger(){
        return singletonInteger++;
    }

    public int getInteger(){
        return singletonInteger;
    }
}
```

기본적으로 applicationContext 시작과 종료까지 싱글톤으로 유지가 되기 때문에 하나의 인스턴스로 여러 호출에서 돌려 먹는다.
```java
@Test
@DisplayName("singleton scope, 인스턴스를 공유하여 호출마다 값이 누적된다.")
void singletonScopeTest(){
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonScopeEx.class);

    SingletonScopeEx bean = ac.getBean(SingletonScopeEx.class);
    bean.addInteger();
    assertEquals(bean.getInteger(), 1); // True

    SingletonScopeEx bean2 = ac.getBean(SingletonScopeEx.class);
    bean2.addInteger();
    assertEquals(bean2.getInteger(), 2); // True

    assertEquals(bean, bean2); // 같은 인스턴스의 bean을 반환한다.
}
```
