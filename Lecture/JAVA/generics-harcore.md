# 제네릭의 활용
> 코딩테스트를 풀다가 `Collections` 클래스의 `sort()` 메소드를 사용하다가 복잡한 제네릭을 쓴 것을 보고 제네릭에 대해 자세히 알아보려 이 글을 적는다.


## 제한된 Generics 클래스
```java
FruitBox<Toy> fruitBox = new FruitBox<Toy>();
fruitBox.add(new Toy());  // OK, 과일상자에 장난감을 담을 수 있다.. 그러나 뭔가 잘못된 것 같지 않은가?
```

* 타입문자로 사용할 타입을 명시하면 한 종류의 타입만 지정할 수 있도록 제한할 수 있지만, 그래도 위의 코드처럼 여전히 모든 종류의 타입을 지정할 수 있다는 것에는 변함이 없다.
* 그렇다면 타입 매개변수 `T`에 지정할 수 있는 타입의 종류를 제한할 수 있는 방법에 대해 알아보자. 

```java
// Fruit의 자손만 타입으로 지정 가능
class FruitBox<T extends Fruit> {
  ArrayList<T> list = new ArrayList<T>();
}
```
* 위와 같이 제네릭 타입에 `extends`를 사용하면 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.

```java
FruitBox<Apple> appleBox = new FruitBox<Apple>(); // OK, Apple은 Fruit의 자손이기 때문에 가능
FruitBox<Toy> toyBox = new FruitBox<Toy>(); // 에러, Toy는 Fruit의 자손이 아니기 때문 불가능
```
* 여전히 한 종류의 타입만 담을 수 있지만, `Fruit` 클래스의 자손들만 담을 수 있다는 제한이 추가 됐을 뿐이다.

```java
FruitBox<Fruit> appleBox = new FruitBox<Fruit>();
fruitBox.add(new Apple());  // OK, Apple은 Fruit의 자손
fruitBox.add(new Grape());  // OK, Grape은 Fruit의 자손
```
* `add()` 매개변수 타입 `T`도 `Fruit`와 자손 타입이 될 수 있으므로, 위와 같이 여러과일을 담을 수 있는 상자가 될 수 있다.

```java
class FruitBox<T extends Eatable> {
  //...
}

interface Eatable {}
```
* 만약 클래스가 아닌 인터페이스를 구현해야 할 때는 implement가 아닌 extends를 사용 한다.

```java
class FruitBox<T extends Fruit & Eatable> {
  //...
}
```
* 타입 `T`를 `Fruit`의 자손이면서 `Eatable`을 구현해야한다면 `&`기호로 연결해준다.