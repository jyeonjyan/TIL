## IS-A 관계
> 상속에 개념에서 나오는 관계이다.  

### ex. 
Dog.class 는 Animal class 를 extends 받았다.  
Dog은 Animal의 하위 개념이라고 볼 수 있다. 이런 경우에 Dog은 Animal에 포함되기 때문에 "개는 동물이다"라고 표현 가능하다.  

자바는 이러한 관계를 IS-A 관계라고 한다. 즉 "Dog `is a` Animal" 과 같이 말 할 수 있다.

다음과 같은 코딩이 가능하다.  
```java
Animal dog = new Dog();
```

하지만 이 반대의 경우, 즉 부모 클래스로 만들어진 객체를 자식 클래스의 자료형으로는 사용할 수 없다.  
```java
Dog dog = new Animal();
```

이 부분을 좀 더 개념적으로 살펴보면
```java
Animal dog = new Dog();
```

위 코드를 읽어보면 "개로 만든 객체는 동물 자료형이다."라고 읽을 수 있다.

또 다음 코드를 보자
```java
Dog dog = new Animal();
```
동물로 만든 객체는 개 자료형이다. 라고 읽을 수 있다.  
뭔가 이상하지 않는가? 동물로 만든 객체는 개 자료형 말고 호랑이 자료형 또는 사자 자료형도 될 수 있지 않는가?

개념적으로 살펴보아도 두번째 코드는 성립할 수 없다. 

자바에서 만드는 모든 클래스는 `Object`라는 클래스를 상속받게 되어 있다.  
사실 우리가 만든 `Animal` 클래스는 다음과 기능적으로 완전히 동일하다.  하지만 굳이 아래 코드처럼 Object 클래스를 상속하도록 코딩하지 않아도 자바에서 만들어지는 모든 클래스는 Object 클래스를 자동으로 상속받게끔 돼 있다.  

```java
public class Animal extends Object {
    String name;

    public void setName(String name){
        this.name = name;
    }
}
```

따라서 자바에서 만드는 모든 객체는 Object 자료형으로 사용할 수 있다.
```java
Object animal = new Animal();
Object dog = new Dog();
```
