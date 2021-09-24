## 스프링 빈 순환 참조
> The dependencies of some of the beans in the application context form a cycle:

### 문제가 왜 생겼나요?
내 app은 한 모델에 대해서 `repository, customRepository, repositoryImpl` 가 있었는데 각 클래스, 인터페이스 관계는 repository 가 customRepository를 parent 로 갖고 customRepository를 Impl가 구현한다.

**순환 종속성 또는 순환 문제는 둘 이상의 Bean 이 생성자를 통해 서로를 주입하려고할 때 발생한다.**

### 해결방안
보통 의존성 주입은 `@Autowired` 또는 `@RequiredArgsConstructor` 를 통한 주입을 할 것이다.

일반적으로 Spring 은 애플리케이션 컨텍스트의 시작 / 부트스트랩 과정에서 모든 싱글톤 Bean 을 즉시 생성한다. 그 이유는 런타임이 아닌 컴파일 과정에서 모든 가능한 오류를 즉시 피하고 감지하는 것이다.

이처럼 문제가 되는 빈들을 주입 받을 때, `@Lazy` 어노테이션을 통해 지연로딩으로 임시방편 문제를 해결한다.

어플리케이션이 실행하는 즉시 초기화가 아닌 필요에 따라 지연 해결 프록시가 주입되는 방식으로 사용한다.

### 결론
`@Lazy` 를 사용해 임시방편으로 문제를 해결하려 하지말고, 스프링 빈들의 관계를 재설계해서 문제를 해결하는것이 중요하다.
