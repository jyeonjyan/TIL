# JubJub Server File Structure
## 💩 JubJub Project Servser File Structure

### 📄 File Structure
```
com.gsm.jubjub
- advice
    - exception
    - ExceptionAdvice.java
- config
    - security
    - MessageConfiguration
    - SwaggerConfiguration
- controller
    - exception
    - v1
- model
    - responce
    - Admin
    - User
- repo
- Application
```
### 1. ```advice/exception/**.java``` 분석
class를 보면 ```extends RuntimeException``` 가 상속 받아진 것을 볼 수 있다.  
> 말 그대로 실행 중에 발생하며 시스템 환경적으로나 인풋 값이 잘못된 경우, 혹은 의도적으로 프로그래머가 잡아내기 위한 조건등에 부합할 때 발생되게 만든다.

ex) test 인자에 a 가 0인 경우 프로그램이 더이상 동작하지 않게 하고 싶다면 RuntimeException을 발생하면 된다.
```java
public static void test(int a){
    if(a == 0){
        throw new RuntimeException("a는 0이 되선 안됩니다.");
    }
}
```