## 자바 리플랙션

### 클래스 정보를 조회하는 다양한 방법
```java
// 이렇게 바로 접근도 가능합니다.
Class<BookRepository> bookRepositoryClass = BookRepository.class;

// app 에 이미 인스턴스가 잡혀 있을 때.
BookRepository bookRepository = new BookRepository();
Class<? extends BookRepository> aClass = bookRepository.getClass();

// 이렇게 forName 을 통해서 경로로 class 를 접근할 수도 있다.
Class<?> aClass1 = Class.forName("com.example.thejava.repository.BookRepository");
```
