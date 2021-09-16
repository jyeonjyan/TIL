## spring bean 과 싱글톤
> 일정시간마다 데이터를 정재하는 스케쥴이 실행된다.  
> 해당 스케쥴을 실행하기 위해 Service Bean 을 수행하는데 초반에는 별 문제가 되지 않다가 데이터가 늘어나면서 속도가 느려지기 시작했다.  

AppRunner.class
```java
@Component
public class AppRunner implements ApplicationRunner {
	@Autowired
	private SampleService sampleService1;

	@Autowired
	private SampleService sampleService2;

	@Override
	public void run(ApplicationArguments args) throws Exception {

	    System.out.println(sampleService1);

	    System.out.println(sampleService2);
	}
}
```

console-result
```java
com.example.sample.service.SampleService@439f37c4
com.example.sample.service.SampleService@439f37c4
```

이러한 현상이 발생하는 이유는 Spring의 Bean은 기본적으로 singleton으로 관리되고 있기 때문이다.  
이는 어찌보면 당연한 일이다. 서버가 실행되는 동안 수도 없이 많이 호출 될 수 있는데, 그때마다 일일히 생성한다면 메모리가 오래 버티지 못할 것이다.


### 호출 될 때마다 생성하기 
그럼 호출 될 때마다 생성하게 할 순 없을까? 가능하다
SampleService 클래스에 Scope 어노테이션을 추가하고 value 에 prototype 을 선언한다.

```java
@Service
@Scope(value = "prototype")
public class SampleService {
	...
}
```

이후 실행해보면 다음과 같이 각각 객체가 생성됨을 확인할 수 있다.

```
com.example.sample.service.SampleService@49860486
com.example.sample.service.SampleService@3e3e51a6
```
