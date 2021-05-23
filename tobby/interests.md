## 관심사의 분리
미래를 준비하는 데 있어 가장 중요한 과제는 변화에 어떻게 대비할 것인가이다.  
가장 좋은 대책은 변화의 폭을 최소한으로 줄여주는 것이다.  
<br>

> 두 명의 개발자에게 동일한 기능 변경을 요청했다고 하자.  
> 그런데 한 명은 단 몇 줄의 코드만 수정하고 그 변경이 나머지 기능에는 전혀 문제를 일으키지 않는다는 것까지 검증해주는데 5분이 걸렸다.  

> 다른 개발자는 동일한 기능을 변경하는데 5시간이 걸린데다.  
> 기존에 잘 동작하던 다른 기능에 오류를 일으킬지도 모른다는 새로운 불안감까지 일으켰다면 괴연 어떤 개발자가 더 미래의 변화를 잘 준비한 것일까?

<br>

잘 설계한 개발자는 **"분리"와 "확장"** 을 고려한 설계가 있었기 때문이다.  
변화 한 번에 한 가지 관심에 집중돼서 일어난다면, 우리가 준비해야 할 일은 한 가지 관심이 한 군데에 집중되게 하는 것이다.

<br>

프로그래밍 기초 개념중에 **"관심사의 분리"** 라는게 있다.  
이를 객체지향에 적용해보면, 관심이 있는 것끼리는 하나의 객체 안으로 또는 친한 객체로 모이게 하고, 관심이 다른 것은 가능한 한 따로 떨어져서 서로 영향을 주지 않도록 분리하는것이라고 생각할 수 있다.

## 중복 코드의 메소드 추출
```java
public void add(User user) throws ClassNotFoundException, SQLException{
    // DB 연결 기능이 필요하면 getConnection() 메서드를 이용하게 한다.
    Connection c = getConnection();
}

private Connection getConnection() throws ClassNotFoundException, SQLException{
    Class.forName("com.mysql.jdbc.Driver");
    Connection c = DriverManager.getConnection(
        "asdfa","spring","book");
        return c;
    )
}
```

