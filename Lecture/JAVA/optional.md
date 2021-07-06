## Optional
> 값이 존재할 수도, 않을 수도 있는 값을 포장한 객체

Null 이 될 가능성을 가진 값을 객체로 감싸는 래퍼 클래스 이다.  
즉, Optional에 포장된 객체는 하나의 원소 혹은 Null 원소가 되는 것을 뜻한다.  
Null을 직접 다루면 위험한 상황이 발생하거나 굉장히 까다롭다.  
이를 Optional 객테에 포장함으로써 유연한 처리가 가능해 진다.  
Null 을 Optional에 포장하게 되면 Null을 값으로 보고 로직을 구현할 수 있다.  

* Null을 직접 다루면 생기는 위험한 상황의 예시
```java
public String phoneNumberFromCustomer(Order order){
    return order.getCustomer().getPhoneNumber();
}
```
이렇게 주문한 고객의 휴대 전화번호를 반환하는 반환하는 기능이 있다.  
만약 `order`이 `null`로 넘어온다면, `order.getCustomer()`은 `null, order.getPhoneNumber()` 도 null 이 된다.  
이렇게, `null`을 return 하게 되면서 호출 부에 NPE를 발생시킬 수 있는 상황이 발생한다.


```java
Order order = null;

String phoneNumber = phoneNumberFromCustomer(order) // null 이 반환
System.out.println(phoneNumber.length()); // NullPointException 발생
```

### Optional 사용하기
1. Optional 객체 생성
   1. `Optional.empty()` 비어있는(null) Optional 객체를 가져온다

    ```java
    Optional<Station> optstation = Optional.empty();
    ```

    2. `Optional.of(T value)` 객체를 담을 Optional 객체를 생성한다. 이 경우 null 이 들어오면 NPE를 발새시킨다.

    ```java
    Optional<Line> optLine = Optional.of(new Line("1호선"));
    ```

    3. `Optional.ofNullable(T value)` 비어있거나 값이 있을 수 있는 객체를 생성한다. (null 여부를 확신할 수 없을 때)

    ```
    Optional<Section> optNullSection = Optional.ofNullable(null); // null
    Optional<Section> optSection = Optional.ofNullable(new Section("잠실역", "몽촌토성역", "850m")); //not null
    ```

2. Optional 값에 접근하기
   1. `.get()`

    ```java
    //값을 가져오고, 비어있는 Optional 객체에 대해서는 NoSuchElementException 예외를 던진다.
    Optional<Station> optStation = Optional.of(new Station("잠실역"));
    Station station = optStation.get();
    ```

    2. `orElse(T order)`

    ```java
    // 비어있는 Optional 객체에 대해서 orElse 로부터 넘어온 인자를 반환한다.
    Optional<Station> optStation = Optional.of(null);
    Station station = optStation.orElse(new Station("잠실역"));
    ```

    3. `orElseGet(Supplier<? Extends T> other`

    ```java
    // 비어있는 Otional 객체에 대해서 orElseGet 으로부터 넘어온 함수형 인자를 통해 생성된 객체를 전달한다.
    Optional<Station> optStation = Optional.of(null);
    Station station = optStation.orElseGet(() -> new Station("임시역"));
    ```

    4. `orElseGet(Supplier<? Extends T> other)`

    ```java
    // 비어있는 Optional 객체에 대해서 orElseThrow 로부터 넘어온 함수형 인자를 통해 예외를 던진다.
    Optional<Station> optStation = Optional.empty();
    Station station = optStation.orElseThrow(UnsupportedOperationException::new);
    ```

### Optional 의 장점
* 명시적으로 변수에 대한 Null 가능성을 표현할 수 있다.
* null 체크를 직접하지 않아도 된다. 
* NPE 발생 가능성이 있는 값을 다룰 필요가 없다.


### Optional 의 단점
* Wrapper 클래스이기 때문에 두 개의 참조를 가지므로 생성비용이 비싸다.
* 직렬화 불가능하기 때문에 인스턴스 필드로 사용하면 안된다.
* 필드로 사용하기 위해 고안된 것이 아니기 때문에 return의 용도로 사용해야한다.