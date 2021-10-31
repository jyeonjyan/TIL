# arrayList item 중복을 제거하는 방법.

### 인덱스
1. arrayList.contains(x)
2. hashCode
3. hashSet

## arrayList.contains(x)
```java
public class Main {
    public static void main(String[] args) {
        ArrayList<String> arrayList = new ArrayList<>();
        arrayList.add("jihwan");

        if (arrayList.contains("jihwan")){
            System.out.println("it is already exist");
        }
    }
}
```

이렇게 이미 arrayList 의 중복을 체킹할 수 있는 `.contains()` 메소드가 존재한다.  
그렇다면 굳이 HashCode를 사용할 필요가 없지 않나 라는 생각을 할 수도 있다. 결론은 전혀 아니다.  

ArrayList의 `contains()` 메소드의 시간복잡도는 `O(n)`이다. List 모든 원소를 하나씩 뒤져서 존재 여부를 탐색해야 하기 때문이다.  
만약 List 의 개수가 무수히 많다면 탐색에 상당한 시간이 소요될 것이다. 이 때 `HashTable`을 사용하면 아주 효과적이다.  

### HashTable
> 이 무언인지는 다들 알고 있을 것이다. 해시테이블은 hashCode() 메소드를 사용하여 주어진 키에 대한 해시 값을 계산하고 내부적으로 이 값을 사용하여 데이터를 저장하기 때문에 접근할 때 훨씬 효율적이다.

## hashCode() 어떻게 작동하는가?
해시코드를 간단하게 말하면 **알고리즘에 의해 생성된 정수** 값이다.  

* equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 그 객체의 hashCode 메소드는 몇번을 호출해도 항상 일관되게 항상 같은 값을 반환해야 한다. (애플리케이션을 다시 실행하면 이 값이 달라져도 상관없다.)
* equals가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.  
* equals가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단, **다른 객체에 대해서는 확실히 다른 값을 반환하는것이 해시테이블 성능이 좋아진다.**

## 잘못된 해시코드 구현
```java
public class MyHash{
    String pk;

    public MyHash(String pk) {
        this.pk = pk;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        MyHash myHash = (MyHash) o;
        return Objects.equals(pk, myHash.pk);
    }

    @Override
    public int hashCode() {
        return Objects.hash(pk);
    }
}
```

이렇게 계속 똑같은 해시코드 값을 반호나하는 메소드를 사용하면 어떻게 될까요?  
해시테이블 하나의 버킷 내에 계속 원소들이 쌓여 리스트 형태로 연결될 것입니다. 그렇다면 해시테이블 검색 시간복잡도 O(1)의 이점을 누리지 못하고 O(n)으로 늘어나게 됩니다.