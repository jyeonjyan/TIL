# 선택정렬

## 선택정렬의 알고리즘 개념
1. 제자리 정렬 알고리즘의 하나이다.
   1. 입력배열 이외에 다른 추가 메모리를 요구하지 않는다.
2. 해당 순서에 원소를 넣을 위치는 이미 정해져 있고, 어떤 원소를 넣을지 선택하는 알고리즘이다.
   1. 첫번재 순서에는 첫번째 위치에 가장 최솟값을 넣는다.
   2. 두번째 순서에는 두번재 위치에 남은 값 중에서의 최솟값을 넣는다.

3. 과정
   1. 주어진 배열 중에서 최솟값을 찾는다.
   2. 그 값을 맨 앞에 위치한 값과 교체한다.
   3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.
   4. 하나의 원소만 남을 때까지 반복한다.

## 선택정렬 회전 및 정렬 결과
<img src="../../img/selection-sort.png">

## 코드 구현 & 설명
```java
public class Main {
    public int[] solution(int[] arr){
        // 0~last 인덱스까지 반복한다.
        for (int i=0; i<arr.length; i++){
            // 자신을 제외한 나머지 index를 비교 반복한다.
            for (int j=i+1; j<arr.length; j++){
                // 만약 i가 더 크다면 비교한 수 j와 자리를 바꾼다.
                if (arr[i] > arr[j]){
                    int tmp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = tmp;
                }
            }
        }
        return arr;
    }
    public static void main(String[] args) {
        Main main = new Main();
        Scanner scanner = new Scanner(System.in);

        int n = scanner.nextInt();
        int[] integers = new int[n];
        for (int i=0; i<n; i++){
            integers[i] = scanner.nextInt();
        }

        System.out.println(Arrays.toString(main.solution(integers)));
    }
}
```

## 선택정렬의 특징
* 장점
  * 자료 이동 횟수가 미리 결정된다. arr[n]의 n만큼
* 단점
  * 안정성을 만족하지 않는다.
  * 즉, 값이 같은 레코드가 있는 경우에 상대적인 위치가 변경될 수 있다.

