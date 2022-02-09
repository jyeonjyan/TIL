# Portable Service Abstraction
> 환경의 변화와 관계없이. 일관된 방식의 기술로의 접근 환경을 제공하려는 추상화 구조

## Spring PSA

Spring은 특정 기술에 직접적 영향을 받지 않게끔 객체를 POJO 기반으로 한번씩 더 추상화한 Layer를 갖고 있으며 이를통해 일관성있는 추상화를 만들어 냅니다.

* `@Transactional` 애노테이션을 사용하면 JPA든 JDBC든 트랜잭션(Commit, Rollblack..)을 관리할 수 있게 됩니다.

* `@Controller` 애노테이션이 붙어 있는 곳에서 `@GetMapping`, `@PostMapping` 애노테이션을 사용해서 요청을 매핑한다.  

`Spring Web`, `Sring WebFlux` 처럼 각 라이브러리 마다 매핑 방식이 따로 있는 것이 아니다. 추상화 계층을 사용해서 개발자들은 해당 애노테이션을 편하게 사용하면 되고, Spring PSA 로 제공되는 기술을 다른 기술 스택으로 간편하게 바꿀 수 있는 확장성을 갖고 있는 것이 PSA이다.

