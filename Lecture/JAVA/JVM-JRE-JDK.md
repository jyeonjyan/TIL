## JVM과 JRE, JDK의 차이는 무엇인가?
1. JVM은 컴파일 된 클래스 파일을 구동하는 역할을 한다. (실제 자바 프로그램 실행자 역할)
2. JRE는 Java Runtime Environment 로 자바 프로그램이 실행될 수 있는 환경을 말한다.

* Class Loader - 컴파일 된 클래스 파일을 메모리에 로딩
* Bytecode verifier - 로딩된 클래스 파일의 정보가 플랫폼에서 실행되는지 문제가 있는지 없는지 검증
* Java Virtual Machine - 검증된 클래스 파일을 플랫폼에서 실행 시켜 준다.


JVM은 JRE의 한 부분으로 JRE는 클래스 파일이 주어지는 동안 자바가 실행될 수 있는 환경을 제공한다.

그렇다면.. JDK 는 Java Development Kit 프로그램 개발을 위한 것을 제공한다.  

컴파일러 - javac, 개발을 위한 데이터베이스, 개발에 필요한 여러가지 툴 셋

<img src="../../img/JVM-JRE.png">