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

```
src/main/java/com.##.##
TestApplication
/domain/Board, BoardRepository
/dto/BoardDTO
/service/BoardService
/controller/BoardController
```
**TestApplication 클래스**
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

