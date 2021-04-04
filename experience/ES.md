# Elastic Search
## ES와 RDBMS
> ES에서 사용되는 데이터 구조를 RDBMS에 대응해 보면 다음과 같이 맵핑된다.

|ES|RDBMS|
|:---:|:---:|
|Index|DataBase|
|Shard|Partition|
|Type|Table|
|Document|Row|
|Field|Column|
|Mapping|Schema|
|Query DSL|SQL|

Elastic Search는 기본적으로 http 프로토콜 접근이 가능한 REST API를 통해 데이터 조작을 지원한다.

|ES HTTP Method|RDBMS SQL|
|:---:|:---:|
|GET|SELECT|
|PUT|INSERT|
|POST|UPADTE, SELECT|
|DELETE|DELETE|
|HEAD(index info)| |

## 역색인
일반적인 DB 에서는 볼 수 없는 개념인 '역색인'은 뭘까?  
'문서 내의 문자와 같은 내용물'의 맴핑 정보를 색인해놓은 것이다.  

<br>

쉬운 예시로 들어보면 일반 색인(forward index)는 책의 목차와 같은 의미이고, 역색인(inverted index)는 책 가장 뒤의 단어 별 색인 페이지와 같다.

## ES 의 특징
ES 는 NOSQL 의 일종으로 분류할 수 있고, 분산처리를 통해 실시간성으로 바른 검색이 가능하다. 특히 기존의 데이터로 처리하기 힘든 대령의 비정형 데이터 검색이 가능하며 전문 검색(full text)검색과 구조 검색 모두를 지원한다.  
기본적으로 검색엔진이지만 MongoDB나 Hbase와 같은 대용량 스토리지도 활용이 가능하다.  

<br>

장점
* 오픈소스 검색엔진이다. 활발한 오픈소스 커뮤니티가 ES를 끊임없이 개선하고 발전시키고 있다.
* 전문검색: 내용 전체를 색인해서 특정 단어가 포함된 문서를 검색할 수 있다.
* 통계분석: 비정형 로그 데이터를 수집하여 통계 분석에 활용할 수 있다. 
* Schemaless: 정형화되지 않은 문서도 자동으로 색인하고 검색할 수 있다.
* RESTful API: HTTP 기반의 RESTful을 활용하고, 요청/응답에 JSON을 사용해 개발 언어, 운영체제, 시스템에 관계없이 다양한 플랫폼에서 활용이 가능하다.
* Multi-tenancy: 서로 상이한 인덱스일지라도 검색할 필드명만 같으면 여러 인덱스를 한번에 조회 할 수 있다.

<br>

**단점**
* 완전 실시간은 아니다: 색인된 데이터는 1초 뒤에나 검색이 가능하다. 내부적으로 commit 과 flush 같은 복잡한 과정을 거치기 때문이다.
* Transaction Rollback을 지원하지 않는다: 전체적인 클러스터의 성능 향상을 위해 시스템적으로 비용 소모가 큰 롤백과 트랜잭션을 지원하지 않는다.
* 데이터의 업데이트를 제공하지 않는다: 업데이트 명령이 올 경우 기존 문서를 삭제하고 새로운 문서를 생성한다. 업데이트에 비해서 많은 비용이 들지만, 이를 통해 불변성이라는 이점을 취한다.