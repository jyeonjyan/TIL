# Configuration error: found multiple declarations of @BootstrapWith for test class

[발생 원인](https://bit.ly/35sy1oa) 해당 자료를 참고하면 해결방법과 다양한 원인들을 쉽게 확인할 수 있다.

## 원인 그리고 해결 방안

나는 아래와 같이 Controller를 테스트하다 에러가 발생했다.
```java
@SpringBootTest
@ContextConfiguration(classes = {Controller.class})
public class ControllerTest {

    @Autowired
    private MockMvc mockMvc;

    ...
}
```


Junit 4에는 `@RunWith(SpringRunner.class)` 사용이 가능하지만, Junit 5 부터는 해당 애노테이션이 비활성화 되었다.  
그래서 그냥 무지성으로 `@SpringBootTest`를 달아서 테스트 했는데 오류가 발생한 것이다.

Junit 5 환경에서는 아래 처럼 바꾸어 시도해보면 될 것 같다.

```java
@ExtendWith(SpringExtension.class) // 변경된 점
@ContextConfiguration(classes = {Controller.class})
public class ControllerTest {

    @Autowired
    private MockMvc mockMvc;

    ...
}
```

## MockMvc 사용하면 @ExtendWith(SpringExtension.class)를 해줘야 하는 이유

@ExtendWith(SpringExtension.class)는 Spring TestContext Framework의 기능을Junit5의 Jupiter 프로그래밍 모델에 통합하는 역할을 한다.

### 서블릿 컨테이너를 Mocking 한다는 의미는..

웹 환경에서 컨트롤러를 테스트하려면 서블릿 컨테이너가 구동되고 DispatcherServlet 객체가 메모리에 올라가야 한다.  
이 때 서블릿 컨테이너를 모킹하면 실제 서블릿 컨테이너가 아닌 테스트용 모형 컨테이너를 사용해서 간단하게 컨트롤러를 테스트할 수 있다.


즉, 컨트롤러만 테스트할 때는 @WebMvcTest를 이외에 컴포넌트들도 테스트하려면 `@AutoConfigureMockMvc`를 사용하자.  
`@SpringBootTest` 로 테스트 했을 때 controller - service 사이에 `autowired` 가 되지 않았다고 오류가 뜨는데 그 이유가 이런 이유이다.  

MockMvc 테스트를 하는데 Controller 단에서 테스트가 그치는게 아니라 Service 혹은 다른 컴포넌트와 유기적인 결합을 통해 결과를 만들어 낸다면 서블릿 컨테이너를 사용해야 하기 때문에 `@AutoConfigureMockMvc` or `@ExtendWith(SpringExtension.class)` 를 추가하여 테스트 하자.