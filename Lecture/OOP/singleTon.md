# 싱글톤 패턴(Singleton Pattern)을 쓰는 이유와 문제점

## 싱글톤 패턴(Singleton Pattern)
싱글톤 패턴이란
> 애플리케이션이 시작될 때 어떤 클래스가 최초 한번만 메모리를 할당하고(static) 그 메모리에 만들어 상요하는 디자인 패턴.  
> 생성자가 여러 차례 호출되더라도 생성되는 객체는 하나고, 최초 생성 이후에는 get 메서드로 받아 씀.

### 결론은 하나의 인스턴스를 생성해 사용하는 디자인 패턴이다.

Company.java
```java
public class Company{
    private static Company instance = new Company();

    private Company(){
    }

    public Company getInstance(){
        return instance;
    }
}
```
CompanyTest.java
```java
// main 생략
Company company = Company.getInstance();
Company company2 = Company.getInstance();

sysout(company);
sysout(company2);
// 둘의 메모리는 같게 잡힌다.
```

## 싱글톤 패턴을 쓰는 이유
고정된 메모리 영역을 얻으면서 한번의 new 생성자로 인스턴스를 사용하기 때문에 메모리 낭비를 방지할 수 있음  
싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기가 쉽다.  
DBCP처럼 공통된 객체를 여러개 생성해서 사용하는 상황에서 많이 사용.  
+ 로딩시간이 현저하게 줄어 성능이 좋아짐.

## 싱글톤 패턴의 문제점
싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들에 간에 결합도가 높아져 "개방-폐쇄 원칙"을 위배하게 된다. (=객체 지향 설계 원칙에 어긋남)

또한 멀티쓰레드환경에서 동기화처리를 안하면 인스턴스가 두개가 생성된다든지 하는 경우가 발생할 수 있음.  
