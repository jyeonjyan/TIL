## 데이터베이스에서 하나 이상의 값에 엑세스 하고 있어서 발생하는 Exception

<ins>발생하는 원인</ins>

```
1. 데이터베이스 값 (ID, 이름 등)이 고유 한 복제 수정인지 확인하십시오.
2. 데이터베이스를 변경하지 않으려면 get (0)을 0 인덱스 값만 반환하십시오.
3. 그렇지 않으면 uniqueResult ()를 List ()로 대체합니다.
```

<ins>발생하는 코드</ins>

<img src="../../img/can't%20return%202%20result.png">

<ins>해결방안</ins>

<img src="../../img/solved%201%20result%20return.png">