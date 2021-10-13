# 인터페이스 [추상 메소드 기본 메소드 스태틱 메소드]

### index
1. 추상 메소드 (abstract method)
2. 기본 메소드 (default method)
3. 스태틱 메소드 (static method)

## 추상 메소드
인터페이스 내부의 `추상 메소드`는 어떠한 class에 implements하면 무조건 구현을 해야한다. 예를들어..  
```java
public interface Method {
    void setName(String name);
    String getName();
}
```

이렇게 `Method`라는 interface안에 두 개의 abstract method를 적었다. (참고로 java 8 부터는 `abstract` 키워드를 삭제해도 무관하다.)  
나중에 `Method` interface를 extends 받은 class 에서는 `setName()`, `getName()` 은 무조건 구현해야하는 것이다.

## 디폴트 메소드
디폴트 메소드는 무조건 적으로 구현할 필요가 없는 클래스이다. (예외는 아래에서 설명..)
```java
public interface Method {
    default void sayBye(){
        System.out.println("Bye");
    }
}
```
이 상황에서는 `Method` 라는 interface를 특정 class에서 extends 받아서 **무조건적으로 구현할 필요는 없다.**(하지만 구현을 한다고 해도 compile 에러는 없음)  

### 예외상황
```java
public interface Method {
    default void sayBye(){
        System.out.println("Bye");
    }
}

public interface MyTools {
    default void sayBye(){
        System.out.println("Bye");
    }
}

public class use extends Method, MyTools{
    // 상속을 받으면 같은 이름의 default method가 존재하게 된다.
    // 이럴 때는 무조건적으로 @Override 해주어야 함.
}
```

## 스태틱 메소드
스태틱 메소드는 인스턴스가 컴파일 시점에서 잡혀 어디서든지 `Class.staticMethod()` 이런식으로 접근이 가능하다. ([오라클 공식문서](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)에서는 아래와 같이 설명한다.)  

<img src="../../img/static-method-in-oracle.png">

static method를 사용하려면 해당 클래스 이름으로 호출하여야 하고, 클래스에 대한 인스턴스를 잡을 필요 없다고 한다.


```java
public interface Method {
    static void startWithMethod(){
        System.out.println("======= start =======");
    }
}

// 사용부
public class FooMain {
    public static void main(String[] args) {
        Method.startWithMethod();
    }
}
```

