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