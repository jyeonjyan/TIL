# 삽입정렬

## 삽입정렬의 알고리즘 개념
* 손안의 카드를 정렬하는 방법
  * 새로운 카드를 기존의 정렬된 카드 사이의 올바른 자리를 찾아 삽입한다.
  * 새로 삽입될 카드의 수만큼 반복하게 되면 전체 카드가 정렬된다.
* 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 정렬을 완성하는 알고리즘
* 매 순서마다 해당 원소를 삽입할 수 있는 위치를 찾아 해당 위치에 넣는다.


## 삽입정렬의 구체적인 개념
* 삽입 정렬은 두 번째 자료부터 시작하여 그 앞의 자료들과 비교하여 삽입할 위치를 지정한 뒤 자료를 뒤로 옮기고 지정한 자리에 자료를 삽입하여 정렬하는 알고리즘이다.
* 즉, 두 번째 자료는 첫 번째 자료, 세 번째 자료는 두 번째와 첫 번째 자료...
* 처음 Key 값은 두 번째 자료부터 시작한다 (i=1 ~ )

<img src="../../img/insertion-sort.png" width="780px">

## 삽입 정렬 코드와 예
```java
public int[] solution(int n, int[] arr){
    int j;
    // 2번째 수 부터 시작한다 index 기준 1
    for (int i=1; i<n; i++){
        int key = arr[i];
        // key 로 잡은 것과 이미 정렬된 곳으로 역순 비교한다.
        for (j=i-1; j>=0 && arr[j] > key; j--){
            arr[j+1] = arr[j];
        }
        arr[j+1] = key;
    }
    return arr;
}
```

## 삽입 정렬 알고리즘의 특징
* 장점
  * 안정한 정렬
  * 레코드 수가 적을 경우 알고리즘 자체가 매우 간단하므로 다른 복잡한 정렬보다 유리하다.
  * 대부분의 레코드가 이미 정렬되어 있는 경우에 매우 효율적일 수 있다.
* 단점
  * 비교적 많은 레코드들의 이동을 포함한다. 
  * 레코드 수가 많고 레코드 크기가 클 경우에 적합하지 않다.