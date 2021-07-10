## 생성자
> 메서드명이 클래스명과 동일하고 리턴 자료형이 없는 메서드를 생성자라고 한다.  

### 생성자의 규칙
* 클래스명과 메서드명이 동일하다.
* 리턴타입을 정의하지 않는다.


생성자는 객체가 생성될 때 호출된다. 여기서 객체는 `new` 라는 키워드로 객체가 만들어질 때이다.

즉, 생성자는 다음과 같이 `new`라는 키워드가 사용될 때 호출된다.
```java
new 클래스명(입력항목)
```

생성자는 메서드와 마찬가지로 입력을 받을 수 있다.  

우리가 만든 생성자는 다음과 같이 입력값으로 문자열을 필요로 하는 생성자다.
```java
public HouseDog(){
    this.setName(name);
}
```

따라서 다음과 같이 `new` 키워드로 객체를 만들때 문자열을 전달해야만 한다.  
```java
HouseDog dog = new HouseDog("happy"); // 생성자 호출 시 String을 전달.
```

아래와 같이 코딩하면 컴파일 오류가 발생할 것 
```java
HouseDog dog = new HouseDog();
```

오류가 발생하는 이유는 객체 생성 방법이 생성자 규칙과 맞지 않기 때문에.  
생성자가 선언된 경우 생성자의 규칙대로만 객체를 생성할 수 있다.  

```java
public class HouseDog extends Dog {
    public HouseDog(String name) {
        this.setName(name);
    } 

    public void sleep() {
        System.out.println(this.name+" zzz in house");
    } 

    public void sleep(int hour) {
        System.out.println(this.name+" zzz in house for " + hour + " hours");
    } 

    public static void main(String[] args) {
        HouseDog dog = new HouseDog("happy");
        System.out.println(dog.name);
    }
}
```
결과는 .. 다음과 같이 출력된다.
```
happy
```