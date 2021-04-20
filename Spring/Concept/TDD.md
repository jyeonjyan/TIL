# TDD (Test-driven Development)

<ins>TDD의 필요성과 동기</ins>
* 객체지향적인 코드 개발
  * 모든 코드가 재사용 성 기반으로 작성되어야 한다.
* 설계 수정 시간의 단축
  * 최초 설계 안을 만족시키며 입출력 구조와 기능의 정의를 명확히 하게 되므로 설계의 구조들을 많이 수정하게 된다.
* 경함은 일찍 찾을 수록 고치는 비용이 적게 든다.
  * 디버깅 시간의 단축시킨다.
* 유지 보수의 용이성
* 테스트 문서의 대체 가능


<ins>들어가기</ins>
<br>

build.gradle
```gradle
dependencies {
    compile group: 'org.slf4j', name: 'slf4j-api', version: '1.7.25' // log를 위해 slf4j 추가
    compile group: 'ch.qos.logback', name: 'logback-classic', version: '1.2.3' // log를 위해 logback 추가
}
```

TestCaseTest.java
```java
public class TestCaseTest {

    private static final Logger logger = LoggerFactory.getLogger(Assert.class);

    private void Assert() {} // 인스턴스 생성을 막기 위해 기본생성자 private 선언

    public static void main(String[] args){
        new TestCaseTest().runTest();
    }

    private void runTest() {
        long sum = 10+10;
        assertTrue(sum == 20);
    }

    private void assertTrue(boolean condition) {
        if(!condition){
            throw new AssertionFailedError();
        }

        logger.info("Test Passed");
    }
}
```
AssertionFailedError
```java
public class AssertionFailedError extends Error {
    public AssertionFailedError() {}
}
```
console
```shell   
09:38:29.993 [main] INFO com.mysema.commons.lang.Assert - Test Passed
```