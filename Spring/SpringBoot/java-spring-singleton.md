## 자바 싱글톤 VS 스프링 싱글톤

### 자바의 싱글톤
1. 생성자를 private 으로 선언 = 외부에서 클래스의 인스턴스 생성 불가
2. static 선언 = 어느 영역이든 접근 가능
3. classLoader 에 의해 어플리케이션 runtime시 단 한번만 인스턴스화 한다.

#### Thread Safety를 보장하는 싱글톤 구현
```java
public class Singleton{

    private static Singleton instance;

       // 생성자
    private Singleton(){}

    // thread safety getter
    public static synchronized Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }

}
```

* 자바 싱글톤은 getInstance() 를 통해서만 인스턴스에 접근할 수 있다.
* 하지만 여러 스레드가 동시에 getInstance() 를 호출한다면 2개의 객체가 생성될 수도 있다.

#### 단점 발생
1. 동시에 호출하는 경우를 고려해 synchronized 키워드를 줬지만, 인스턴스가 초기화되고나면, 필요없는 synchronized이 계속 발생해 효율이 저하
2. locking overhead 로 인한 성능 저하

```java
public static Singleton getInstance() { 
    if (instance == null) { 
      synchronized (Singleton.class) { 
        if(instance==null) { 
          instance = new Singleton(); 
        } 
      } 
    } 
    return instance; 
  } 
```

`synchrozied` 키워드를 if 안으로 넣기만해도 초기화시점에만 진행되어 퍼포먼스가 향상된다. 

### 스프링 싱글톤 패턴
스프링 싱글톤은 **스프링 컨테이너**에 의해 구현된다.  
스프링에서 컨테이너 내 특정 클래스에 대해 @Bean 이 정의되면, 스프링 컨테이너는 그 클래스에 대해 단 하나의 인스턴스를 만든다.  
이 공유 인스턴스는 설정 정보에 의해 관리되고 bean 이 호출될 때마다 스프링은 생성되어 있는 공유 인스턴스를 리턴 시킨다.  

**공유 인스턴스의 생성시점은 스프링 컨테이너에 따라 달라진다.**

빈 팩토리는 최초 호출시점에 인스턴스를 생성하고, 어플리케이션 컨텍스트는 미리 모든 공유 인스턴스를 초기화해 두었다가 호출될 때 바로 리턴한다.  

**어떠한 호출에도 단일 인스턴스를 리턴시켜주기 때문에 thread safety가 보장된다.**


#### 정리
**1. 자바 싱글톤은 classLoader 에 의해 구현되고, 스프링 싱글톤은 스프링 컨테이너에 의해 구현된다.**
2. 자바 싱글톤의 scope는 코드 전체이고, 스프링의 싱글톤은 scope 해당 컨테이너 내부이다.
3. 스프링에 의해 구현되는 싱글톤 패턴은 thread safety 가 자동으로 보장된다. 자바 싱글톤은 개발자의 로직에 따라 보장 할 수도 있고, 아닐 수도 있다.