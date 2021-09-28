## 나만의 @애노테이션 만들기 그리고 그 기능
> 애노테이션을 만들고 응용해보자.

### 애노테이션 생성
```java
public @interface MyAnnotation {
}
```

#### 여기서 MyAnnotation을 컴파일된 바이트코드를 인식하게 하고 싶다면?
```java
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
}
```
> `@Rentention` 애노테이션 추가하기.

#### 애노테이션에 메서드를 추가하고 싶다면?
```java
public @interface MyAnnotation {
    String name() default "jihwan";
    int age() default 18;
}
```
> 그 메서드의 기본 값을 설정하고 싶다면 `defualt` value 와 같이 지정해주면 된다.

#### 만약 애노테이션에 메서드는 만들고 기본값을 주지 않았다면?
> 사용시점에서 `@MyAnnotation(name = "", age = int)` 이렇게 줘야 한다.

### 애노테이션 사용하기
BookRepository.java
```java
@Repository
@MyAnnotation
public class BookRepository {
    private final String bookName = "hello world";
    private final String bookAuthor = "jihwan";
    public final String coast = "12000 won";
}
```
> 이렇게 내가 원하는 클래스 위에 달아서 할 수 있다. 


### 애노테이션을 이용한 리플랙션

1. 해당 클래스에 달려 있는 애노테이션 조회하기
```java
System.out.println("============getBookRepository Annotation=================");
Arrays.stream(BookRepository.class.getAnnotations()).forEach(System.out::println);
```

**+a** [상속 된 애노테이션 까지 조회하기] - `BookMain extends BookRepository`  
원하는 애노테이션에 `@Inheritance` 를 달아서 자식 클래스의 getAnnotation() 을 조회해보자!
```java
System.out.println("============getBookMain Annotation=============");
Arrays.stream(BookMain.class.getAnnotations()).forEach(System.out::println);
```

2. 필드의 애노테이션 가져오기
> 아래와 같이 필드에 애노테이션을 지정하고..
```java
@MyAnnotation
public final String coast = "12000 won";
```

먼저 `getDeclaredFields` 로 모든 필드를 다 끌어오고 그 모든 필드에 `.getAnnotations()` 를 한 뒤 출력한다.
```java
System.out.println("============getBookRepository Field Annotation=============");
    Arrays.stream(BookRepository.class.getDeclaredFields()).forEach(f -> {
    Arrays.stream(f.getAnnotations()).forEach(System.out::println);
});
```