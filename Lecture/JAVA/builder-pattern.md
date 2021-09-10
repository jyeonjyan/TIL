## 빌더 패턴을 사용해야만 하는 이유

#### 생성자와 수정자로 구현된 User 클래스를 바탕으로 왜 생성자나 수정자보다 빌더를 써야하는지 이해해보도록 하자
```java
@NoArgsConstructor 
@AllArgsConstructor 
public class User { 
    private String name; 
    private int age; 
    private int height; 
    private int iq; 
}
```

#### 빌더 패턴의 장점
1. 필요한 데이터만 설정할 수 있다. 
2. 유연성을 확보할 수 있다.
3. 가독성을 높일 수 있다.
4. 불변성을 확보할 수 있다.


#### 1. 필요한 데이터만 설정할 수 있음
> User 객체를 생성해야 하는데 age 라는 파라미터가 필요 없는 상황이라고 가정하면,  
> 우리는 age에 더미를 넣어주거나 age 가 없는 생성자를 새로 만들어 줘야 한다.  
> 이런 작업이 한두번이면 괜찮지만, 반복적인 경우에는 시간 낭비로 이어지게 된다. 하지만 이러한 것을 빌더를 이용하면 동적으로 처리 가능하다.


```java
User user = User.builder()
        .name("지환이는 개발자") 
        .height(180) 
        .iq(150).build();
```

그리고 이렇게 필요한 데이터만 설정할 수 있는 빌더의 장점은 테스트용 객체를 생성할 때 용이하게 해주고, 불필요한 코드의 양을 줄이는 등의 이점을 안겨준다.

### 2. 유연성을 확보할 수 있다.
User에 몸무게를 나타내는 새로운 변수 weight 를 추가했다고 가정하면..  
하지만 이미 다음과 같은 생성자로 객체를 만드는 코드가 있다고 해보자.

```java
// ASIS 
User user = new User("망나니개발자", 28, 180, 150) 
// TOBE 
User user = new User("망나니개발자", 28, 180, 150, 75)
```

그러면 우리는 새롭게 추가되는 변수 때문에 기존의 코드를 수정해야 하는 상황에 직면하게 된다.
기존에 작성된 코드의 양이 방대하다면 감당하기 어려울 수 있지만 빌더 패턴을 이용하면 새로운 변수가 추가되는 등의 상황에 직면하더라도 기존의 코드에 영향을 주지 않을 수 있다.

이외에도..  

3. 가독성을 높일 수 있고
4. 불변성을 확보 할 수 있다. 
많은 개발자들이 수정자(Setter) 패턴을 흔히 사용한다. 하지만 Setter 를 구현하는 것은 불필요하게 확장 가능성을 열어두는 것이다.
Open-Closed 법칙에 위반되고, (개방폐쇄의원칙) 불필요한 코드 리딩을 유발한다. 그렇기 때문에 클래스 변수를 final로 선언하고 객체의 생성은 빌더에게 맡기는 것이 좋다.

```java
@RequiredArgsConstructor 
@Builder
public class User { 
    private final String name; 
    private final int age;
    private final int height;
    private final int iq; 
}
```

다른 사람과 협업을 하거나 변경에 대한 요구 사항이 많은 경우라면 빌더 패턴을 이용해야 하는 이유를 더욱 극명하게 느낄 수 있다.  