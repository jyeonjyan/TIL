# SpringBoot 란?

### **SpringBoot 소개**
***
> 1. 단독 실행 가능한 어플리케이션 개발 가능.
> 2. 내장된 톰켓, Jetty, UnderTow등의 서버를 이용해서 별도의 설치없이 실행.
> 3. 자동화된 설정방식을 제공.
> 4. 기존 스프링 환경에서 설정했던 XML 없이 단순 자바수준의 설정방식을 제공.
> 5. maven, gradle의 환경을 제공.
> 6. nosql, AWS 환경도 자동 제공.

**기존의 Spring 개발 환경 보다 편리함** <br>
**WAS를 start 하고 대기 시간 없이 run 가능**

**SpringBoot =** ```SpringFrameWork``` + ```Embedded HTTP Server``` - ``` XML <bean> ```

<br>

### **Project Setting**
********************************
> 1. Use IntelliJ IDEA
> 2. Go [start.spring.io](https://start.spring.io/)
> 3. Maven Project
> 4. Java Language
> 5. SpringBoot 2.4.1
> 6. Packaging Jar
> 7. Java 11 version
> 8. Dependencies  
> [```SpringBootDevTools```, ```Lombok```, ```Processor```, ```JPA```, ```H2 Database```]

<br>

### **Project Structure**
***

```java
src/main/java/com.##.##
TestApplication
/domain/Board, BoardRepository
/dto/BoardDTO
/service/BoardService
/controller/BoardController
```
**```TestApplication``` 클래스**
> main 매서드가 선언.  
> main 매서드는 ```SpringApplication.run``` 메서드를 호출해서 웹 애플리케이션을 실행하는 역할.

**```@SpringBootAplication``` 의 구성**
> * ```@EnableAutoConfiguration```: 일부가 자동으로 완료됨.
> * ```@ComponentScan```: 의존성 주입을 간편하게 해줌.
> * ```@Configuration```: 자바 기반 설정 파일로 인식, less XML Loading.

**```src/main/resources``` 디랙터리**
> ```/static/..```:해당 폴더에는 css, fonts, images, plugin, scripts 등의 정적 리소스 파일이 위치합니다.   
> ```/templates/..```: 화면과 관련된 파일을 관리하는 것으로 생각할 수 있습니다.  
> ```/application.properties```: 해당 파일은 웹 애플리케이션을 실행하면서 자동으로 로딩되는 파일입니다.

**```src/test/java```디렉터리**
> 해당 디렉터리의 com.board 패키지에는 BoardApplicationTests 클래스 존재  
> 해당 클래스를 사용하면 스프링과 달리 곧바로 테스트가 가능.

**```build.gradle```**
> Maven 사용하면 라이브러리 버전 문제, 충돌 문제, 종속적인 문제 등.  
> 요즘에는 그레이들을 선호하는 추세.  
> 추가한 라이브러리는 ```project and External Dependencies``` 에서 확인.

**MVC 패턴**
> **M(odel)**
> * 데이터를 처리하는 영역(비즈니스로직).
> * 데이터베이스와 통신하여 사용자가 원하는 데이터를 가공하는 역할.
> 
> **V(iew)**
> * 사용자가 보는 화면을 의미, Html, 타임리프 처리.
> * 뷰 = 화면 = 사용자
> 
> **C(ontroller)**
> * 모델과 뷰의 중간 단계.
> * 사용자의 요청을 처리할 어떠한 로직을 호출.
> * 호출한 결과를 사용자에게 "전달" 하는 역할.

