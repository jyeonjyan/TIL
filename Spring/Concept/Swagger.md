## **Swagger란?**
**📌 Swagger: REST API로 개발시 문서를 자동으로 만들어주는 프레임워크**
***

## **🧐 배경**
> 작은 규모의 단일 서버가 아니라면 보통의 경우.  
> **사용자 -> WEB, Mobile -> API -> DB**  
> 이런 구조에서는 API서버가 어떤 스펙으로 데이터를 주고받는지 문서가 꼭 필요.

**"But!" api 문서를 document로 관리하면 너무 귀찮은 일, 그래서 Swagger 등장**

> ### **API → Swagger Definition → Server/Client code**
> ### **API → Swagger Definition → API Document**

****

## **Swagger**
### **🛠 Swagger 에서 공식적으로 지원하는 툴 리스트**

> 1. Swagger Core
>     * Swagger definition을 생성하는 library
> 
> 2. Swagger CodeGen
>    * Swagger definition을 이용하여 ```Server/Client code```를 생성하는 Command-line-tool
> 
> 3. Swagger UI
>     * REST API document web 제공
> 
> 4. Swagger Editor
>     * YAML/JSON 형태의 Swagger definition을 편집하는 editor

### **Swagger 기능**
> 1. 작성된 API 기반으로 ```Swagger Definition Generation```
> 2. 작성된 API 기반으로 API Document ```Web Service -> Swagger-UI```
> 3. Swagger Definition을 기반으로 ```API Document Web Service -> Swagger-UI```
> 4. Swagger Definition 을 기반으로 ```Test Code Generation -> Swagger CodeGen```

### **Swagger 환경설정**
> Springfox를 이용한 Swagger 연동 방법 입니다.  
> 1. maven dependency에 Springfox 라이브러리 추가.
> 2. Swagger Configuration 파일을 추가.
> 3. Swagger UI를 resource 에 추가.
> 4. Swagger JSON 생성 확인.

**```maven dependency```에 ```springfox``` 라이브러리 추가**
```xml
<!--  For Swagger core, ui  -->
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
<version>2.5.0</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.5.0</version>
</dependency>
```

**```SwaggerConfiguration```파일 추가**
> 가급적 config 관련 package에 추가하도록 한다.
```java
// Swagger 2.0 version을 사용 annotation
@EnableSwagger2                               
// 설정 파일 annotation
@Configuration                                
// WebMvc 사용 설정 annotation
@EnableWebMvc    
// REST API 를 찾을 Package 지정 annotation
@ComponentScan({ "ga.jjeaby.rest.api" })     
public class SwaggerConfiguration {
  @Bean
  public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2).select().apis(RequestHandlerSelectors.any())
                   .paths(springfox.documentation.builders.PathSelectors.regex("/.*")).build();
   }
}
```

**Swagger UI를 resource에 추가**
> AOP 로 인증 처리를 하는 경우 path를 인증 AOP에서 Exclude 하도록 한다.
```xml
<mvc:resources location="classpath:/META-INF/resources/swagger-ui.html" mapping="swagger-ui.html" /> 
<mvc:resources location="classpath:/META-INF/resources/webjars/" mapping="/webjars/**" /> 
<mvc:interceptors>
	<mvc:interceptor>
	   <mvc:mapping path="/**" />
	   
    	   <!-- For Swagger UI, Rest V1 Except Auth -->
	   <mvc:exclude-mapping path="/swagger-ui.html" />
	   <mvc:exclude-mapping path="/swagger-resources" />
	   <mvc:exclude-mapping path="/webjars/** " />
	   <mvc:exclude-mapping path="/v2/api-docs" />
....
```

## Spring 서버 구동시 Swagger JSON 생성 확인
* 서버 구동시 /v2/api-docs가 Mapping 되는지 Log를 확인.
* localhost:8080/../api-docs에 접속하면 json 파일이 열리는지 확인.