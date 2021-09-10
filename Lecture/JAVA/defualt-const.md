## 기본 생성자가 필요한 이유 (with Jackson ObjectMapper + Reflection)

아직 리플렉션이 무엇인지도 모르는 상태여서 ObjectMapper와 Reflection에 관해 찾아보기로 했다.  
또, 기본 생성자가 진짜 필요한가? 궁금증이 들기도 했다.

### Serialize
* 직렬화
* Object to JSON
* `getter` 를 사용

### Deserialize
* 역직렬화
* JSON to Object
* default constructor 를 사용한다. -> 불변 객체로 못 만든다.

### Reflection
리플렉션은 접근 제어자와 상관 없이 클래스 객체를 동적으로 생성하는 자바 API 이다.  
참고로, 자바는 정적 언어이다. 따라서, 컴파일 시점에 객체 타입을 결정한다.

리플렉션을 찾아보다가 아래와 같은 문장을 마주쳤다.
> Almost all frameworks require a default(no-argument) constructor in your class because these frameworks use reflection to create objects by invoking the default constructor

리플렉션은 무조건 기본 생성자가 필요하다는 내용이다.  
왜냐하면 기본생성자로 객체를 생성하기 때문이다 (객체 초기화의 역할도 해줌)

**왜 기본 생성자로 생성하지해서 찾아보았다.**
> Java Reflection 이란?  
> 구체적인 클래스 타입을 알지 못해도, 그 클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API

> 자바에서 제공하는 리플렉션(Reflection)은 C, C++과 같은 언어를 비롯한 다른 언어에서는 볼 수 없는 기능이다. **이미 로딩이 완료된 클래스에서 또 다른 클래스를 동적으로 로딩(Dynamic Loading)하여 생성자(Constructor), 멤버 필드(Member Variables) 그리고 멤버 메서드(Member Method) 등을 사용할 수 있도록 한다.**

> java Reflection이 가져올 수 없는 정보 중 하나가 바로 생성자의 인자 정보들이다. 따라서 기본 생성자 없이 파라미터가 있는 생성자만 존재한다면 java Reflection이 객체를 생성할 수 없게 되는 것이다.

이래서 기본 생성자가 필요하다.  
그런데 생성자의 인자 정보를 못 가져오면 인자가 있는 생성자로 객체 생성을 어떻게 하지? 궁금해서 또 찾아봤다.

> Spring Data JPA 에서 Entity에 기본 생성자가 필요한 이유도 동적으로 객체 생성 시 Reflection API를 활용하기 때문이다. Reflection API로 가져올 수 없는 정보 중 하나가 생성자의 인자 정보이다. 그래서 기본 생성자가 반드시 있어야 객체를 생성할 수 있는 것이다. 기본 생성자로 객체를 생성만 하면 필드 값 등은 Reflection API로 넣어줄 수 있다.

결론적으로 기본 생성자로 객체를 생성하고,  
클래스의 필드 정보를 가져올 수 있으니, 객체 생성 후에 필드 값을 할당하는 것이다.

### 간단하게 + a 생성자를 만들어 주는 이유
1) 인스턴스 생성시 **필드에 초기값으로 부여**하기 위해
2) 인스턴스 생성에 필요한 초기화 명령을 실행하기 위해


#### 생성자 작성 방법
1. 반환형을 사용하지 않는다 -> 값을 return 할 수 없다.
2. 생성자의 이름은 반드시 클래스명과 동일하게 작성한다.
3. 생성자는 오버로딩이 가능하다 -> 생성자는 여러 개 선언 가능하다.




