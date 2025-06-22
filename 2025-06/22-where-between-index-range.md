# MySQL where between index range 를 이용한 성능개선

## 사례 소개

과거에 모든 고객을 대상으로 조건에 부합한다면 푸시 알림을 쏴주는 배치를 개발한적이 있다. (query A)

```sql
select distinct(u.device_id)
from promotion p
join user u on u.user_id = p.user_id
where p. promotion_type = 'a';
```

조건은 이보다 훨씬 복잡했지만, 대충 이런 쿼리였고 promotion 은 전체 유저 대상으로 N개가 생성되는 구조였기 때문에 user 데어터보다 훨씬 많은 양을 갖고 있었다.  
결론만 보자면 위 쿼리를 다음과 같이 바꿔서, 배치에서 DB 부하를 덜어내어 안정적으로 결과를 낼 수 있도록 개선했었다.  

전체 user 를 대상으로 조건을 탐색해야하기 때문에 일단 최소 user_id 와 최대 user_id 를 리졸빙을 했다. (query B)
```sql
select min(user_id), max(user_id)
from user;
```

그런다음, chunk 를 `10000` 으로 줘서 `1 ~ 10000`, `10001 ~ 20000` 이런식으로 range 를 옮겨가며 user_id 끝까지 탐색할 수 있도록 개선했었다. (query C)
```sql
select distinct u.device_id
from promotion p
join user u on u.user_id = p.user_id
where p.promotion_type = 'a'
  and p.user_id between :begin_user_id and :end_user_id;
```

이때 이렇게 변경한데는 다음과 같은 이유가 있었다.

* query A 를 batch 에 돌려보면 너무 무거워서 그런지 query timeout 이 났다. 
  * 그래서 좀 더 적은 범위로 여러번 가져오면 되지 않을까? 라는 생각을 했다.
  * 만약 성공적으로 가져온다고 하더라도 select 되는 row 수가 많아서 batch 에서 OOM 이 발생했을거라 적당한 모수를 가져왔어야 했다.
    * 끊어서 가져온다고 한다면 `LIMIT X` 를 쓰면 되지 않을까 했는데, "전체 user 를 빠뜨림 없이" 조건을 대입해봐야 했기 때문에 단순히 LIMIT 만으로는 만족 X.

당시 query B & query C 를 해주는 helper 함수가 `bucketBeteen` 이라는 이름으로 존재했었고, fundermental 한 이점을 완벽하게 이해하지는 못한채 **이게 정배니깐** 사용했었다.  
실제로 적용했을때 안정적이고 나름 빠르게 배치가 수행되었고, 이제 와서보니 생각드는것도 있고 해서 이 방법이 먹혔던 fundermental 한 근거를 대보려고 한다.


## OFFSET LIMIT 쿼리도 방법이 될 수 있었을 것 같다.

