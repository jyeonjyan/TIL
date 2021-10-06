## Spring Bean Life Cycle 

### Java Bean vs Spring Bean
Java Bean 은 데이터를 표현하는 것을 목적으로 하나 자바 클래스이다.  
특별한 점은 없고 Java Bean 규약에 맞춰서 만든 클래스를 뜻한다.  

```
Java Bean 규약
1. 기본생성자가 존재해야 한다.
2. 모든 멤버변수의 접근제어자는 private 이다.
3. 멤버변수마다 getter/setter가 존재해야 한다.
4. 외부에서 멤버변수에 접근하기 위해서는 메소드로만 접근 할 수 있다.
5. 직렬화가 가능해야한다.
```

#### 직렬화
직렬화란 시스템 내부에서 사용하는 객체 혹은 데이터르르 외부의 시스템에서도 사용할 수 있도록 변환시키는 것을 말한다.  
자바에서는 JVM의 Heap 영역에 상주한 객체를 byte 형태로 변환시키거나, byte 형태를 다시 자바 객체로 변환하는 것(역직렬화)를 말한다.

> CSV, JSON format으로 자바 객체를 변경시키는 것도 직렬화하는 것이라고 볼 수 있다.

Serializable interface를 implements한 클래스는 직렬화 할 수 있다.


```java
import java.io.Serializable;

// 직렬화가 가능하도록 Serializable 인터페이스를 구현
public class Person implements Serializable {
    // 모든 멤버변수의 접근자는 private
    private String name;
    private int age;
    private String address;

    // 기본생성자가 있어야한다.
    public Person() {
    }

    // 기본생성자가 있다면 매개변수가 있는 생성자가 있어도 무방함
    public Person(String name, int age, String address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }

    // 각 멤버변수에 접근할 수 있는 getter/setter가 있어야한다.
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

Spring은 뷰 영역에 데이터를 출력하고 싶을 때 Java Bean 규약에 맞춰 만들어진 객체를 사용하고, 객체들을 외부 저장소에 저장하고 전송할 때 사용한다.

### Spring Bean 
Spring Bean은 spring Framework의 Container에 등록, 생성, 조회 관계설정이 되는 객체이다. 
Java Object와 동일하지만 IoC 방식으로 관리되는 오브젝트를 뜻한다.  

Spring Bean은 Java Bean과 달리 별다른 생성 규칙은 없다.

### IoC/DI 란?
