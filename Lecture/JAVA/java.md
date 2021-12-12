# Java 기술면접 이정도는 대답하자.

## 객체지향에 대해서 설명하시오.

컴퓨터 페러다임 중 하나로, 프로그래밍에서 **필요한 데이터를 추상화시켜 상태와 행위를 가진 객체를 만들고** 그 객체들 간의 유기적인 상호작용을 통해 로직을 구성하는 프로그래밍 방법이다.

## SOLID 원칙에 대해 설명해주세요.

우선 SOLID 원칙에는 SRP, OCP, LSP, DIP, ISP 가 존재합니다.
이 원칙을 적용해 유지 보수와 확장이 쉬운 소프트웨어를 만드는데 이 원칙들을 적용할 수 있습니다.

## Java의 구동원리를 설명하세요.

우선 Java는 컴파일 언어이며, JVM의 classLoader 가 .java를 class로 바꿔주는 역할을 합니다. [소스코드를 바이트코드로 생성 해준는 역할을 합니다.]  

인터프리터적인 기능도 수행한다고 할 수 있는게 내부에 JIT 컴파일러가 존재해 runtime 에서 적잘한 시간에 네이티브 언어를 번역한다.

## 쓰레드란 무엇이고, 싱글쓰레드와 멀티쓰레드 차이를 설명해주세요.

쓰레드는 프로세스를 처리하기 위한 일련의 단위인데요.  
프로세스 1을 처리하기 위해 단일 쓰레드가 동작하면 싱글쓰레드, 2개 이상의 쓰레드가 동작한다면 멀티쓰레드라고 볼 수 있습니다.

쓰레드의 특징을 한가지만 얘기하자면 순서를 보장하지 않는다는 특징이 있습니다.

## 클래스는 무엇이고, 객체는 무엇인지 설명해주세요.

클래스는 객체를 만들기 위한 설계도, 혹은 그 틀을 말하고, 객체는 소프트웨어 세계에 구현할 대상을 말합니다.

## 인터페이스와 추상클래스의 차이점은 무엇인지 설명해주세요.

추상클래스는 상속을 통해 자손클래스에서 완성하도록 유도하는 클래스이며, 미완성 설계도(IS~A) 라고 표현할 수 있습니다.

인터페이스는 기본 설계도(HAS~A)로 표현할 수 있으며, 다중상속이 가능하다는 차별점이 존재합니다.

## 직렬화가 무엇인지 설명하세요.

자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트 형태로 데이터 변환하는 기술과 바이트로 변환된 데이터를 다시 객체로 변환할 수 있는 기술을 아울려 이야기 합니다.  

DB의 직렬화 역직렬화와 로직이 유사하다고 할 수 있습니다.

## Call by Value와 Call by Reference의 차이에 대해 설명해주세요.

인자로 받은 값을 복사해서 처리하냐, 인자로 받은 값은 값의 주소를 참조하여 값의 영향을 주냐의 얘기인데.  

개인적인 생각이지만, shallow copy와 deep copy의 개념과 비슷하다고 생각합니다.

## Checked Exception과 Unchecked Exception의 차이를 설명해주세요.

Exception 핸들링이 필요하냐 필요없냐의 차이 입니다.  
Checked 가 무조건 Exception 핸들링이 필요하고, Unckecked 가 하지 않아도 됩니다.

## JVM의 역할을 설명하세요

JVM 은 Java Virtual Machine 으로 자바 가상 머신을 줄여 부른것이다.  
JVM 은 classLoader를 통해 class 파일들을 JVM 으로 로딩한다.  
JVM 은 메모리를 효율적으로 관리해주며, 클래스 파일을 해석하여 실질적인 수행까지 이루어지게 한다.

## JDBC란 무엇인가요?

Java Database Connectivity 로 자바에서 DB 프로그래밍을 하기 위해 사용되는 것이다.  
Hibernate orm framework 또한 내부적으로 JDBC API를 사용합니다.

## Hibernate란 무엇인가요?

하이버네이트는 자바 언어를 위한 ORM 프레임워크예요.  
JPA의 구현체로, JPA 인터페이스를 구현하며, 내부적으로 JDBC API를 사용해요.

JPA는 관계형 데이터베이스와 객체의 페러다임 불일치 문제를 해결할 수 있다는 점과 영속성 컨텍스트를 제공이 큰 특징이예요.

객체를 불러올 때, 연관된 객체 또한 함께 불러오기 때문에, SQL에 의존적이지 않은 개발을 할 수 있어요.  
즉, SQL 중심이 아닌 객체 중심의 개발이 가능해요.

## Data JPA 보다 Querydsl로 쿼리를 처리하는걸 선호하는 이유에 대해 알려주세요.

문자가 아닌 코드로 쿼리를 작성함으로써, 컴파일 시점에 문법 오류를 쉽게 확인할 수 있어요.   
자동 완성 등 IDE의 도움을 받을 수 있어요.  
동적인 쿼리 작성이 편리해요.