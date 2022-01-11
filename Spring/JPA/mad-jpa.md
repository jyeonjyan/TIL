# 알고나면 화가나는 JPA 오류 유형들
> 오류를 발견하고 원인을 🙄 찾아내는 과정은 매우 고통스럽지만.. 알고나면 너무 허무한 JPA 관련 오류들을 모아볼게요

## 무작정 Table 관련 속성들을 추가하고 변경할 때.

### 발생한 에러
```
org.hibernate.tool.schema.internal.ExceptionHandlerLoggedImpl handleException WARN: GenerationTarget encountered exception accepting command : Error executing DDL " drop table Order if exists" via JDBC Statement
```

### 해결방안
1. 기존에 있던 TABLE 들을 모두 날려주고(DROP) 실행해 본다.

<img src="../../img/drop-table.png" width="500px">

2. [H2 데이터베이스 버전 확인하기.](https://inf.run/4uLo)
3. [데이터베이스 예약어랑 테이블, 칼럼 이름이랑 겹치는지 확인한다.](https://bit.ly/3telu1m)

### 내가 겪은 것

나는 이 글을 작성하는 이 시점에 3번째 해결방안을 검토해서 해결했다.  
난 테이블 이름이 `Order` 여서..(Order는 `OrderBy` sql의 예약어임) `@Table` 애노테이션에 name 옵션을 줘서 해결했다.

<img src="../../img/db-예약어-주의.png" width="450px">

## 무작정 hibernate 버전을 업데이트 했을 때.

### 에러를 유발한 코드
```xml
<dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.4.13.Final</version>
</dependency>
```

### 발생한 에러
```
The following method did not exist: 'void org.hibernate.annotations.common.reflection.ReflectionManager.reset()'
```

이건 Hibernate Error 인데, 우리는 JPA 를 이미 dependency로 가지고 있기 때문에 `hibernate-core` 와 `hibernate-annotations` 사이에 dependency conflict 를 내는것임.  
아무튼 무작정 hibernate 관련 dependency는 추가하지 말것 !!

## 무지성 import 를 했을 때.

### 에러를 유발한 코드
```java
import com.jpa.just.wrong.Member; // 잘못된 entity import

public interface MemberRepository extends JpaRepository<Member, Long>{}
```

### 발생한 에러 (IDE 시점의 에러)
```
Inferred type 'S' for type parameter 'S' is not within its bound; 
```

처음보는 에러라 당황했는데, 내가 작업하고 있는 환경이 `MemberEntity` 를 두개 가지고 있는 환경이여서 import가 잘 못 해서 발생한 문제였다.  