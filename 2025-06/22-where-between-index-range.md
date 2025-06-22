# MySQL BETWEEN vs OFFSET 실전 배치 쿼리 최적화와 Index Range Scan의 위력

## TL;DR
* 쿼리 성능 문제 때문에 OFFSET 대신 BETWEEN 으로 chunking 처리
* Index Range Scan은 Index Scan보다 훨씬 효율적
* 순차 접근은 OS/DB/CPU 차원에서 프리패치 최적화 이점을 얻을 수 있음

## 사례 소개

과거에 모든 고객을 대상으로 조건에 부합한다면 푸시 알림을 쏴주는 배치를 개발한적이 있다. (query A)

```sql
select distinct(u.device_id)
from promotion p
join user u on u.user_id = p.user_id
where p. promotion_type = 'a';
```

조건은 이보다 훨씬 복잡했지만, 대충 이런 쿼리였고 **promotion 은 전체 유저 대상으로 N개가 생성되는 구조였기 때문에** user 데어터보다 훨씬 많은 양을 갖고 있었다.  
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

당시 query B & query C 를 해주는 helper 함수를 `bucketBeteen` 이라는 개념으로 정의해 사용했고, 안정적이고 빠르게 잘 돌아갔다.


### OFFSET LIMIT 을 이용한 페이징 쿼리도 방법이였지 않았을까?

```sql
SELECT DISTINCT u.device_id
FROM promotion p
JOIN user u ON u.user_id = p.user_id
WHERE p.promotion_type = 'a'
ORDER BY p.user_id
LIMIT {page_size} OFFSET ({page_number} - 1) * {page_size};
```

* page_size 1000 page_number 가 1 이면 : LIMIT 1000 OFFSET 0; (id 1 부터)
* page_size 1000 page_number 가 2 이면 : LIMIT 1000 OFFSET 1000; (id 1001 부터)

이런 모양의 쿼리가 나오고, 전체 user 를 빠뜨림 없이(+ 중복 X) 처리하기 위해 user_id 로 정렬이 들어가고 따라서 성능이 더 좋을 수는 없을 것 같다.  
그럼 promotion_id 를 기준으로 페이징 하면 되지 않냐? 라는 질문이 생기는데, 이렇게 되면 구간 마다 user_id 가 여러번 나올 수 있어서 중복제거 작업을 한 번 더 해줘야 한다. (app에서의 부담 증가)  

**따라서 내 요구사항에는 `bucketBetween` 으로 처리하는것이 DB 와 APP 의 부담을 최소화하여 처리하는데 최고의 방법이라는것을 알 수 있다.**

## OFFSET 쿼리와 BETWEEN 쿼리 성능 비교

문득, OFFSET 으로 beginId 를 주는것과 BETWEEN 으로 beginId 를 주는것 사이에 성능 차이가 있을까? 라는 생각이 들었다.  
그래서 다음과 같이 테이블을 만들고, 1000만 개의 데이터를 넣는 프로시져를 실행시켜 성능을 비교해봤다.

```sql
-- test 테이블 생성
CREATE TABLE articles (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100),
    content TEXT,
    created_at DATETIME
) ENGINE=InnoDB;

-- 천만개 rows 삽입
DELIMITER $$
CREATE PROCEDURE insert_articles()
BEGIN
    DECLARE i INT DEFAULT 0;
    WHILE i < 10000000 DO
        INSERT INTO articles (title, content, created_at)
        VALUES (CONCAT('Title ', i), REPEAT('Content ', 10), NOW());
        SET i = i + 1;
    END WHILE;
END$$
DELIMITER ;

CALL insert_articles();
```

OFFSET 페이징 쿼리의 성능 테스트

```sql
EXPLAIN ANALYZE
SELECT * FROM articles ORDER BY id LIMIT 10 OFFSET 100000;

-- 결과
-> Limit/Offset: 10/100000 row(s)  (cost=1146 rows=10) (actual time=19.1..19.1 rows=10 loops=1)
    -> Index scan on articles using PRIMARY  (cost=1146 rows=100010) (actual time=0.0134..17.2 rows=100010 loops=1)
```

BETWEEN 쿼리의 성능 테스트 
```sql
EXPLAIN ANALYZE
SELECT * FROM articles WHERE id BETWEEN 100001 AND 100010;

-- 결과
-> Filter: (articles.id between 100001 and 100010)  (cost=2.55 rows=10) (actual time=0.00917..0.0135 rows=10 loops=1)
    -> Index range scan on articles using PRIMARY over (100001 <= id <= 100010)  (cost=2.55 rows=10) (actual time=0.00804..0.0119 rows=10 loops=1)
```

두 쿼리 사이에서 성능 차이가 있는것을 확인할 수 있었다.

1. 실행시간
   1. OFFSET: actual time 19ms
      1. 100010 개를 읽고 10 개를 반환 함. (버려질 앞의 N개를 스캔해서 비효율 발생)
   2. BETWEEN: actual time 0.004ms
      1. 10 개를 읽고 10 개를 반환 함.
2. Index scan vs Index range scan
   1. Index scan 은 B+Tree 의 리프 노드를 처음부터 순차적으로 훑음
      1. OFFSET 이 큰 수 일수록, 느리고 메모리/디스크 캐시에 부담
   2. Index range scan 은 B+Tree 에서 해당 범위로 바로 점프해서 시작
      1. 빠르고 locality-friendly

OFSSET 도 `where id > X` 와 같이 동작하면 좋겠지만, OFFSET 은 select 결과에서의 순서상 몇 번째의 개념이기 때문에 `where id > X` 와 같이 동작할 수 없다.  


### CPU locaility 최적화
저장장치에서의 data retrieval 성능을 알아보면 항상 **CPU 캐시 Locality 최적화**에 대한 언급이 있는데, 다음과 같은 궁금증이 생겼다.

> 1 ~ 1000 까지 id 가 있다고 가정하고 between 으로 range 를 1 ~ 100 으로 Page A 조회했을때 buffer 에 있다고 예상되는건 1 ~ 100 인데,  
> 왜 다음번에 조회할 range인 101 ~ 200 Page B도 CPU cache locaility에 최적화 이점을 받을 수 있다는거지?

* Page A에서 row 1~100을 읽을 때 메모리에 로드
* Page B가 뒤이어 미리 로드된다면 row 101~200은 메모리 상에서 연속
* CPU는 cache line 단위로 이 메모리 영역을 자동 프리페치(예측)
* 그래서 다음 range인 Page C도 locality에 잘 맞음
 
디스크 / OS / InnoDB는 sequential access를 감지하면, 다음 블록도 미리 올림  
디스크 → buffer pool → CPU cache까지의 전체 흐름에서, 인접 범위를 순차적으로 요청할 경우, 메모리 접근성과 CPU cache 효율이 동시에 좋아질 가능성이 높음

### 결론
index range scan 할 수 있는 방법인 `BETWEEN beginId` 을 사용하자!