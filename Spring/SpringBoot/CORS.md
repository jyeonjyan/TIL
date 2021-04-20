# 스프링 부트에서 CORS 문제 해결하기

### <ins>특정 컨트롤러나 요청에 대해서 Cross Origin 허용하기</ins>

``@CrossOrigin`` 사용
```java
@RestController
@CrossOrigin(origins = "http://localhost:63342") //해당 origin 승인하기
@RequestMapping("/api/books")
public class VocaTestApiController {
    ...
}
```

전역 설정을 통해서 Corss Origin 허용하기
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfigurerImpl implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**") //모든 요청에 대해서
                .allowedOrigins("http://localhost:8080"); //허용할 오리진들
    }
}
```

### <ins>Origin 이란?</ins>
* Origin 은 아래 세가지의 조합이다.
  * URL 스키마(http, https)
  * Hostname(도메인 네임)
  * 포트 번호

<ins>즉, 호스트네임이 같더라도 다른 포트번호를 사용하면 다른 Origin 이라는 것</ins>

### <ins>그래서 CORS는 뭐고 SOP는 뭔데</ins>
* SOP(Same Origin Policy) - 악의적인 문서로부터 잠재적인 공격을 방어하기 위한 웹 보안 정책
* CORS(Cross Origin Resource Sharing) - 안전한 Origin 들에 대해서 SOP 보안정책을 풀어주기 위한 방법

