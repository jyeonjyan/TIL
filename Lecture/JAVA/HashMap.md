## 자바 HashMap을 효과적으로 사용하는 방법
> HashMap 은 편하고 빠르다. 하지만 어떻게 하면 효율적으로 잘 사용할지 몸부림치는 순간도 많다.

여러분이 편의점(원문은 식료품점)을 운영하고 있고, 여러분의 가게에서 많은 종류의 상품을 다루고 있다고 하자. 그 많은 상품들은 각자 이름과 가격이 있다. 이 모든 상품을 모두 기억하고 있는 것은 어려운 일이다. 상품들의 정보를 노트에 기록하는 중에도 상품을 팔아야되며 또 그것들의 가격을 찾는 등의 행위를 해야한다.

만약에 노트에 이름순으로 적어 놓지 않는다면 매번 상품에 대한 정보를 찾는 시간이 오래걸릴 것이다. 알고리즘적으로 표현하자면 O(n)의 복잡도를 가진다고 한다. 하지만 이름순으로 유지가된다면 이진탐색을 할수가 있으므로 (log n)의 시간에 찾기가 가능하다. 즉 O(log n)이다. 알다시피 O(log n)은 O(n)보다 적게 시간이 걸린다.

비록 O(log n) 이 상당히 적은 시간이 소요된다 하더라도 여전히 어느 정도의 시간은 걸린다. 시간이 전혀 걸리지 않는다면 그것이 최선일 것이다. 아마 여러분이 모든 상품의 이름과 가격을 기억할 수 있고 손님이 상품의 이름을 말하는데로 바로 가격을 대답할 수 있는 경우일 것이다.

이제, `HashMap` 에 대하여 정의 하자. `HaskMap` 은 고정시간을 제공하는 `key-value` 데이터 구조이다. O(1) 복잡도는 `get`과 `push` 동작에 동일하다.

ex_1
```java
package Hashmap;

import java.util.HashMap;
import java.util.Set;

public class HashmapEx {
    public static void main(String[] args) {
        var productPrice = new HashMap<String, Double>();
        // add value
        productPrice.put("Rice", 6.9);
        productPrice.put("Flour", 2.7);
        productPrice.put("Sugar", 4.3);
        productPrice.put("Egg", 1.9);

        //get value
        Double egg = productPrice.get("Egg");
        System.out.println("The price of Egg is: " + egg);

        /**
         * HashMap에서 모든 Key와 value를 print 하기
         * print 문은 간결한 Lambda 표현식 forEach를 선호한다.
         */
        Set<String> key = productPrice.keySet();
//        for (String x: key){
//            System.out.println(x);
//        }
        key.forEach(System.out::println);
    }
}
```

### Key 가 존재하는지 알고 싶은 경우
1. 피보나치 수열은 재귀를 보여주는 경우에 유명한 수열이다. (수열의 공식)
```
f(n) = f(n-1) + f(n-2)
```

메모지네이션을 이용한다면, 쉽게 계산할 수 있다.
```java
import java.math.BigInteger;
import java.util.HashMap;
import java.util.Map;
public class Fibonacci {
    private Map<Integer, BigInteger> memoizeHashMap = new HashMap<>();
    {
        memoizeHashMap.put(0, BigInteger.ZERO);
        memoizeHashMap.put(1, BigInteger.ONE);
        memoizeHashMap.put(2, BigInteger.ONE);
    }
    private BigInteger fibonacci(int n) {
        if (memoizeHashMap.containsKey(n)) {
            return memoizeHashMap.get(n);
        } else {
            BigInteger result = fibonacci(n - 1).add(fibonacci(n - 2));
            memoizeHashMap.put(n, result);
            return result;
        }
    }
    public static void main(String[] args) {
        Fibonacci fibonacci = new Fibonacci();
        for (int i = 0; i < 100; i++) {
            System.out.println(fibonacci.fibonacci(i));
        }
    }
}
```

코드에서 피보나치 번호가 이미 `memoizeHashMap` 에 있는지 여부를 체크한다. 만일 있다면, 계산할 필요가 없고 단지 Key를 가지고 get 을 하면 된다. 아닌 경우에는 계산한다.

위의 코드에서는 100까지 피보나치 번호를 빠르게 제공한다.

map에 해당 key가 존재한다면 `put()` 메서드가 value를 key로 대체한다.

### computeIfAbsent() 과 puIfAbsent()
`containsKey()` 대신에 `computeIfAbsent()` 메서드를 사용한다면 우리는 이것을 좀 더 짧고 간결하게 만들 수 있습니다.

```java
private BigInteger fibonacci (int n) {
  return memoizeHashMap.computeIfAbsent(n, 
           (key) -> fibonacci(n - 1).add(fibonacci(n - 2)));
}
```
이 메소드는 두 개의 입력값을 받습니다. 첫번째는 key이고, 두번째는 key를 사용하여 차례대로 value를 반환하는 함수형 인터페이스 입니다. map에 key가 있으면 값을 반환한다는 아이디어입니다.  
그렇지 않다면, value를 계산하고 map에 추가를 한후에 value를 돌려줄것입니다. 이렇게 하면 코드가 보다 간단해 지고 짧아집니다.

그러나 value를 직접 얻을 수 있는 `putIfAbsent()` 라는 메서드도 있습니다.
```java
productPrice.putIfAbsent("Fish", 4.5);
```