# @NoargsConstructor(AccessLevel.PROTECTED) 와 @Builder

## 1. @NoargsConstructor(AccessLevel.PROTECTED)를 사용하는 이유
```java
@NoargsConstructor(AccessLevel.PROTECTED)
```
나도 Entity나 DTO를 사용할때 `@NoargsConstructor(AccessLevel.PROTECTED)` 를 많이 사용하는 편이다.  
이유는 기본 생성자 접근 제어를 PROTECTED 로 설정해놓게 되면 **무분별한 객체 생성에 대해 한번 더 체크할 수 있기 때문이다.**

예를 들어 User라는 Class는 `name, age, email` 정보를 `@NotNull` 하게 가지고 있어야만 되는 경우에, 좋은 수단이 된다.  
만약 기본 생성자 권한이 `public` 이라면 아래 상황이 발생하게 된다. 

```java
@Getter
@Setter
@NoArgsConstructor
public class User {
    private String name;
    private Long age;
    private String email;
}


/// Main.java
public static void main(String[] args) {
    User user = new User();
    user.setName("testname");
    user.setEmail("test@test.com");
    
    /// age가 설정되지 않았으므로 user는 완전하지 않은 객체
}

```
> 참고로 나는 `@Setter` 를 쓰는 것도 피하는 편이다 해당 내용은 아래서 자세히 다루는걸로 하겠다.

User의 멤버변수들을 설정할 방법이 없으니 Setter를 만들어서 값을 설정하지만  
실수로 User의 멤버변수(age)인 `setAge()`를 누락할 경우 객체는 불완전한 상태가 되어버린다.

하지만 아래와 같이 변경하면 IDE단계에서 누락을 방지할 수 있게 되어 훨씬 수월하게 작업할 수 있게 되고 이는 엄청난 가치입니다.
```java
/// User.java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {
    private String name;
    private Long age;
    private String email;
    
    public User(Long age, String email) {
    	/// 파라미터가 두 개인 경우 name은 default 설정
        this.name = "blank name";
        this.age = age;
        this.email = email;
    }
}


/// Main.java
public static void main(String[] args) {
    User user = new User(15, "test@a.com");
    
    /// 기본 생성자가 없고 객체가 지정한 생성자를 사용해야하기 때문에
    /// 무조건 완전한 상태의 객체가 생성되게 된다.
}
```

## 2. @NoargsConstructor(AccessLevel.PROTECTED)와 @Builder를 함께 사용하면 더 간단해질까?
> 보통 `@Builder` 를 사용하게 되면 조금 더 자유롭게 객체를 생성할 수 있게 된다.

```java
@Builder
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {
    private String name;
    private Long age;
    private String email;
}
```
하지만 저 두 어노테이션을 함께 사용하면 compile 시점에서 오류가 발생합니다.

### `@Builder`는 Class(Type)가 Target일 경우에 생성자 유무에 따라 아래와 같이 작동합니다.
* 생성자가 없는 경우: 모든 멤버변수를 파라미터로 받는 기본 생성자 생성
* 생성자가 있을 경우: 따로 생성자 생성 x

위 과정 이후에 모든 멤버 변수를 설정할 수 있는 `Builder Class`를 생성합니다.
그런데 만약 `@NoargsConstructor(AccessLevel.PROTECTED)` 라는 생성자가 있는 상태에서 `@Builder`를 사용하게 되면 아래와 같이 컴파일 됩니다.

```java
public class User {
    private String name;
    private Long age;
    private String email;


    /// @NoArgsConstructor(access = AccessLevel.PROTECTED)로 생성된 생성자
    protected User() {}

    public static User.UserBuilder builder() {
        return new User.UserBuilder();
    }

    public static class UserBuilder {
        private String name;
        private Long age;
        private String email;

        UserBuilder() {
        }

        public User.UserBuilder name(String name) {
            this.name = name;
            return this;
        }

        public User.UserBuilder age(Long age) {
            this.age = age;
            return this;
        }

        public User.UserBuilder email(String email) {
            this.email = email;
            return this;
        }

        public User build() {
            /// 일치하는 생성자가 없다.
            return new User(this.name, this.age, this.email); 
        }
    }
}
```

User에는 기본 protected 생성자ㅏ 이미 존재해서 따로 생성자를 만들지는 않았지만.  
build()를 보면 모든 파라미터를 생성자로 객체를 build 하려하는 과정에서 알맞는 생성자를 찾을 수 없게 되었습니다. `new User(this.name, this.age, this.email); ` 

## 3. @NoargsConstructor(AccessLevel.PROTECTED)와 @Builder를 함께 사용할 수 없을까?
> 불가능하다고 하더라도 `@NoargsConstructor(AccessLevel.PROTECTED)` 와 `@Builder`는 의미있는 객체를 생성하기 위한 좋은 방법이자 제약조건입니다.

### 해결방법
1. @AllArgsConstructor
```java
@Builder
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@AllArgsConstructor
public class User {
    private String name;
    private Long age;
    private String email;
}
```
`Builder.class`의 `build()`메소드의 return에 해당하는 생성자가 없기 때문에 발생한 일입니다.  
그러면 모든 멤버변수를 받는 생성자 `@AllArgsConstructor`를 만들어주면 됩니다.

2. 생성자에 설정하는 `@Builder`
```java
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {
    private String name;
    private Long age;
    private String email;

    @Builder
    public User(Long age, String email) {
        this.name = "test_name";
        this.age = age;
        this.email = email;
    }

    @Builder
    public User(String name, Long age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
}
```

생성자별로 설정되는 멤버변수 내용을 정의하고 생성자에 `@Builder`를 설정하게되면 해당 생성자를 사용하는 **Builder가 생성되어 의미있는 객체만 생성할 수 있게 됩니다.**