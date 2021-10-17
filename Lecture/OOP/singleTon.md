# 싱글톤 패턴(Singleton Pattern)을 쓰는 이유와 문제점
[이 리드미에서는 java 싱글톤과 spring 싱글톤의 차이를 알 수 있어요!](../../Spring/SpringBoot/java-spring-singleton.md)

## 싱글톤 패턴(Singleton Pattern)
* 인스턴스가 오직 1개만 생성되어야 한다.  
* 레지스트리 같은 설정 파일의 경우 객체가 여러 개 생성되면 설정 값이 변경될 위험이 있다.  
* 인스턴스가 1개만 생성되는 특징을 가진 싱글톤을 이용하면 하나의 인스턴스를 메모리에 등록해서 여러 스레드가 동시에 해당 인스턴스를 공유하여 사용하게끔 할 수 있으므로, 요청이 많은 곳에서 사용하면 효율을 높일 수 있다.  
* 하지만 싱글톤을 사용할 때 주의할 점이 있다. 바로 동시성 문제를 고려해서 설계해야 한다는 점이다.

### 결론은 하나의 인스턴스를 생성해 사용하는 디자인 패턴이다.
1. 기본생성자는 private 하게
2. 메소드는 static 하게 

Company.java
```java
public class Company {
    // static 인스턴스
    private static final Company company = new Company();
    // 기본생성자
    private Company(){ }
    // 인스턴스 getter
    public static Company getInstance(){
        return company;
    }
}
```

CompanyTest.java
```java
public class Example {
    public static void main(String[] args) {
        Company aInstance = Company.getInstance();
        Company bInstance = Company.getInstance();

        if(aInstance == bInstance){
            // 해당 출력문이 실행된다.
            System.out.println("Yeeess! it's the same");
        }
    }
}
```

## Eager Initialization | 이른 초기화, Thread-safe
Company.java
```java
// Eager Initialization
private static final Company company = new Company();
```

#### 여기서 static 을 통해 인스턴스를 생성하는 이유는?
static 키워드의 특징을 이용해서 클래스로더가 초기화 하는 시점에서 정적 바인딩(컴파일 시점에서 성격이 결점됨)을 통해 인스턴스를 메모리에 등록하여 사용하기 위해서이다.

이른 초기화 방식은 클래스 로더에 의헤 **클래스가 최초로 로딩 될 때 객체가 생성되기 때문에 Thread-safe 하다.**

Thread-safe를 신경써야 하는 이유는 멀티 스레딩 환경에서도 동장 가능하게끔 해야하기 때문이다.

## Lazy Initialization with synchronized (동기화 블럭, Thread-Safe)
이 방식은 `synchronized` 키워드를 이용한 게으른 초기화 방식인데 메서드에 동기화 블럭을 지정해서 Thread-safe를 보장한다.

**결론적으로 동적 바인딩을 한다는 것이다. 인스턴스가 필요한 시점에 요청하여 인스턴스를 생성한다.**

```java
public class SingleTon {
    private static SingleTon singleTon;
    // private 기본 생성자
    private SingleTon(){ }
    // synchronized 키워드를 통한 lazy 동기화 블럭
    public static synchronized SingleTon getInstance(){
        if (singleTon == null){
            singleTon = new SingleTon();
        }
        return singleTon;
    }
}
```

동기화 블럭을 지정한 lazy 초기화 방식은 Thread-safe 하지만, 인스턴스가 생성되었든 안되었든 무조건 동기화 블럭을 거치게 돼 있다는 것입니다.

**또한 `synchronized` 키워드를 사용하면 성능이 100개 가량 떨어집니다.**

## 싱글톤 패턴을 쓰는 이유
고정된 메모리 영역을 얻으면서 한번의 new 생성자로 인스턴스를 사용하기 때문에 메모리 낭비를 방지할 수 있음  
싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기가 쉽다.  
DBCP처럼 공통된 객체를 여러개 생성해서 사용하는 상황에서 많이 사용.  
+ 로딩시간이 현저하게 줄어 성능이 좋아짐.

## 싱글톤 패턴의 문제점
싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들에 간에 결합도가 높아져 "개방-폐쇄 원칙"을 위배하게 된다. (=객체 지향 설계 원칙에 어긋남)

또한 멀티쓰레드환경에서 동기화처리를 안하면 인스턴스가 두개가 생성된다든지 하는 경우가 발생할 수 있음.  
