# [디자인 패턴] Design Pattern
도움을 주셔서 감사합니다 🙇🏻‍♂️
> https://gmlwjd9405.github.io/2018/07/06/design-pattern.html


### Index
1. 디자인 패턴의 개념
2. 디자인 패턴의 구조
3. 디자인 패턴의 분류
4. 디자인 패턴의 종류


### 디자인 패턴이란?
* 소프트웨어를 설계할 때 특정 맥락에서 자주 발생하는 고질적인 문제들이 또 발생했을 때 재사용할 수 있는 훌륭한 해결책
* "바퀴를 다시 발명하지 마라"
* 이미 만들어져서 잘 되는 것을 처음부터 다시 만들 필요가 없다는 의미이다.
* 패턴이란?
  * 각기 다른 소프트웨어 모듈이나 기능을 가진 다양한 응용 소프트웨어 개발을 할 때도 서로 간에 공통되는 설계 문제가 존재하며 이를 처리하는 해결책 사이에도 공통점이 있는 것.
  * 패턴은 공통의 언어를 만들어주며 팀원 사이의 의사소통을 원활하게 해주는 아주 중요한 역할을 한다.

### 디자인 패턴의 구조
1. Context - 콘텍스트
   1. 문제가 발생하는 여러 상황을 기술한다. 즉, 패턴이 적용될 수 있는 상황을 나타낸다.
   2. 경우에 따라서는 패턴이 유용하지 못한 상황을 나타내기도 한다.
2. Problem - 문제
   1. 패턴이 적용되어 해결될 필요가 있는 여러 디자인 이슈들을 기술한다.
   2. 이때 여러 제약 사항과 영향력도 문제 해결을 위해 고려해야 한다.
3. Solution - 해결
   1. 문제를 해결하도록 설계를 구성하는 요소들과 그 요소 사이의 관계, 책임, 협력 관계를 기술한다.
   2. 해결은 반드시 구체적인 구현 방법, 언어에 의존적이지 않으며 다양한 상황에 적용할 수 있는 일종의 템플릿.


### 디자인 패턴의 종류
* GoF 의 디자인 패턴
  * GoF(Gang of Fout)라 불리는 사람들
    * 에리히 감마, 리차드 헬름, 랄프 존슨, 존블라시디스
    * 소프트웨어 개발 영역에서 디자인 패턴을 구체화하고 체계화한 사람들
    * 23가지 디자인 패턴을 정리하고 각각의 디자인 패턴을 생성, 구조, 행위 3가지로 분류

* GoF 디자인 패텅의 분류
<img src="../img/GoF-Design.png">

1. 생성(Creational)패턴
   1. 객체 생성에 관련된 패턴
   2. 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 `유연성 제공`
2. 구조(Structual)패턴
   1. 클래스나 객체를 조합해 더 `큰 구조`를 만드는 패턴
   2. 예를 들어 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스 생성, `객체를 묶어` 새로운 기능을 제공
3. 행위(Behavioral)
   1. 객체가 클래스 사이의 알고리즘이나 `책임 분배`에 관련된 패턴
   2. 한 객체가 혼자 수행할 수 없는 작업을 여래 개의 객체로 어떻게 `분배`하는지, 또 그렇게 하면서도 객체 사이의 `결합도를 최소화`하는 것에 중점을 둔다.


