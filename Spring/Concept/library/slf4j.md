# SLF4J 이용하여 로그 남기는 방법 with Logback
## SLF4J 란?
SLF4J는 로깅 Facade(퍼사드)이다. 로깅에 추상 레이어를 제공하는 interface 모음이다.  
여러 로깅 라이브러리를 하나의 통일된 방식으로 사용할 수 있는 방법을 제공한다.  

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6WdgT%2FbtqyfsqQv2y%2Fx95Ep6sWxeEgDLJTZr783K%2Fimg.gif">

application은 SLF4J를 이용하여 로깅 라이브러리가 어떤 것이든 같은 방법으로 로그를 남길 수 있게 되는 것이다.  

나중에 더 많은 라이브러리가 생기더라도, application의 코드를 변경할 필요가 없다.  

## Spring 에서 SLF4J 활용하기
- 우선 스프링은 기본적으로 아파치 재단의 commons-logging을 이용해 로그를 남긴다.  
- Logback 라이브러리로 로그를 남기려면 commons-logging이 사용되지 않도록 설정을 해야 한다.
- 그런데 Spring은 내부적으로 commons-logging을 찾아 로깅 하려고 해서, commons-logging을 사용하지 못하도록 설정하면 ClassNotFoundException이 발생한다.

> 이 문제를 해결하기 위해 SLF4J 에서 제공하는 jcl-over-slf4j 라이브러리를 추가한 것이다.  

<br>
jcl-over-slf4j는 commons-logging과 동일한 구조를 가지며, SLF4J를 사용하여 로그를 남긴다.  

## Logback 설정하기

SLF4J는 로그를 남기기 위한 공통 인터페이스이다.  
SLF4J로 logback 라이브러리를 이용하여 로그를 남기는 것이므로, 실제 로그를 남기는 것은 logback 라이브러리다.
<br>

### 로그의 다섯 가지의 레벨 (아래쪽으로 내려 갈수록 심각한 오류)
> 1. trace
> 2. debug
> 3. info
> 4. warn
> 5. error