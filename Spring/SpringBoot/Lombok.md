## Lombok 사용시 주의할 점.
*  ``@AllArgsConstructor``, ``@RequiredArgsConstructor`` 사용금지


   * 필자도 개발의 편의를 위해 ``@AllArgsConstructor`` 와 ``@RequiredArgsConstructor``를 즐겨 사용했다. 하지만 나의 별 생각 없는 생성자 자동 생성 어노테이션이 치명적 버그를 만들게 되게 될 수 있었다. 

    ```java
    @AllArgsConstructor
    public static class Order {
        private long cancelPrice;
        private long orderPrice;
    }
    
    // 취소금액 5,000원, 주문금액 10,000원
    Order order = new Order(5000L, 10000L); 
    ```

    이 클래스에 대해 자동으로 `canclePrice`, `orderPrice` 순서로 인자를 받는 생성자가 만들어진다.

    그런데 개발자가 보기에는 Order 클래스 인데 `cancelPrice`가 `orderPrice` 보다 위에 있는 것이 마음에 안들어 순서를 바꾼다면


    ```java
    @AllArgsConstructor
    public static class Order {
        private long orderPrice;
        private long cancelPrice;
    }
    ```

    이렇게 된다면 개발자도 인식하지 못하는 사이에 생성자 파라미터의 순서를 필드 선언 순서에 맞춰 `orderPrice`, `cancelPrice` 로 바꿔버리게 된다.

    게다가 이 두 필드는 동일한 `Type`으로 생성자 호출 시 인자 순서를 변경하지 않음에도 어떠한 오류도 발생하지 않는다. 

    하지만, 이 아무런 오류 없이 잘 동작하는 코드는 실제로 입력된 값은 바뀌어 들어가게 된다.

    ```java
    // 주문금액 5,000원, 취소금액 10,000원. 취소금액이 주문금액보다 많아짐!
    Order order = new Order(5000L, 10000L); // 인자값의 순서 변경 없음
    ```

    이런식으로 `lombok` 어노테이션을 사용하지 않는 것을 권장한다.

    대신, 생성자(constructor)를 직접 만들고 필요한 경우에는 직접 만든 생성자에 `@Builder` 어노테이션을 사용하는 것을 권장한다. 

    파라미터 순서가 아닌 이름으로 값을 설정하기 때문에 리팩터링에 유연하게 대응이 가능하다.