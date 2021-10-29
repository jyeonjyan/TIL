# Counting Sort, 계수 정렬
## 소개
`Counting-sort`는 계수 정렬 알고리즘으로 `O(n)`의 시간복잡도를 갖습니다.  
반면, 일반적인 상황에서 가장 빠른 정렬 알고리즘인 `Quick-sort`의 평균 시간복잡도는 `O(nlogn)`입니다.

`Counting-sort`는 어떻게 빠를까요? 빠르다면 왜 대부분의 경우에서 `Counting-sort`를 쓰지 않고 `Qick-sort`를 쓸까요?

`Counting-sort`가 어떻게 동작하는지 이해하고 나면 의문에 대한 답을 찾을 수 있을 것입니다.

## 정렬과정
[5, 8, 4, 5, 3, 5] 이와 같은 수열 A를 정렬해야한다고 했을 때 `Counting-sort`는 각 숫자가 몇 번 등장하는지 세어줍니다.

> 수열 A의 최대 값이 8이기 때문에 배열을 0~8까지로 만들어 줘야 함.  
> 벌써 부터 비효율적이라는 생각이 든다 숫자 사이에 등장하지 않는 수 까지 무의미한 순회를 해야하기 때문이다.

코드로 구현하는게 빠를 것 같다.

```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // MAX 는 입력 받을 수 있는 최대 값.
        int MAX = 5;
        // Out of index error를 피하기 위해 +1
        int[] arr = new int[MAX+1];
        int[] arrB = new int[MAX+1];
        int[] count = new int[MAX+1];
        int[] countSum = new int[MAX+1];

        // count 배열을 모두 0으로 초기화
        for (int i=0; i<=MAX; i++){
            count[i] = 0;
        }

        // 수열 A에 입려되는 수는 MAX_NUM 이하여야 한다.
        for (int i=1; i<=MAX; i++){
            arr[i] = scanner.nextInt();

            // 숫자 동작 횟수 세기
            count[arr[i]]++;
        }

        // 누적 합 구성
        countSum[0] = count[0];
        for (int i=1; i<=MAX; i++){
            countSum[i] = countSum[i-1]+count[i];
        }

        // 뒤에서 부터 arr를 순회한다.
        for (int i=MAX; i>=1; i--){
            arrB[countSum[arr[i]]] = arr[i];
            countSum[arr[i]]--;
        }

        // 수열 A를 정렬한 결과인 수열 B를 출력한다.
        for (int i=1; i<=MAX; i++){
            System.out.println(arrB[i]);
        }
    }
}
```

## 정리
코드로 정리해 놓은 것이 어느정도 의문에 대한 답이 되었길 바란다.  

`Counting-sort` 알고리즘의 시간복잡도는 O(n)으로 `Quick-sort` 보다 훨씬 유리해보인다. 그러나 세상에 공짜는 없다는 말처럼 `Counting-sort`는 대부분의 상황에서 엄청난 메모리 낭비를 한다.

**아무튼 카운팅 배열은 정렬하는 숫자가 특정한 범위 내에 있을 때 사용하는 것이 좋다.**