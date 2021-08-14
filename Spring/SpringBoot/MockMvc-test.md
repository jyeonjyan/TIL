## Spring Boot JUnit 5 MockMvc Test
> [도움을 주셔서 감사합니다 🙇🏻‍♂️](https://gofnrk.tistory.com/74)


## AbstractControllerTest
> 저는 test/testConfig 패키지에 따로 뺏습니다.

설정하는 방법
* AutoConfiguration
* Customizing

## AutoConfiguration
> 아주 간단하게 테스트 할 때 사용하는 방법 mockMvc 객체를 사용해서.

```java
@SpringBootTest
@AutoConfigureMockMvc
public abstract class AbstractControllerTest {
    
	@Autowired
	protected MockMvc mockMvc;

}
```

## Customizing
오토컨피그로 설정하면 MockMvc 를 커스텀하기 어려움이 있다.  
그래서 MockMvcBuilders 를 활용하여 직접 MockMvc 인스턴스를 생성한다.

```java 
@SpringBootTest
public abstract class AbstractControllerTest {
    protected MockMvc mvc;

    abstract protected Object controller();

    /**
     * MockMvcBuilders 를 활용한 MockMvc 커스텀 메서드
     * 1. addFilter: UTF-8 인코딩을 해주지 않으면 테스트 수행 시 body가 깨진다.
     * 2. alwaysDo: 항상 콘솔에 테스트 결과를 찍어줘라.
     * 3. alwaysExpect: 항상 200 코드를 반환하기를 기대한다.
     *
     * @author: 전지환
     */
    @BeforeEach
    private void setup(){
        mvc = MockMvcBuilders.standaloneSetup(controller())
                .addFilter(new CharacterEncodingFilter(StandardCharsets.UTF_8.name(), true))
                .alwaysDo(print())
                .alwaysExpect(status().isOk())
                .build();
    }
}
```

## 사용방법
> 내가 테스트 하고 싶은 controller + extends + AbstractControllerTest


### 주의사항
* 새션발급로직을 추가(@BeforeEach)하면 JWT 추가적인 header 검증이 필요가 없다.