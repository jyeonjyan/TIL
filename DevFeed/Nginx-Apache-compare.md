# Nginx, 그리고 Nginx가 Apache 보다 성능이 좋은 이유 

## 논외
요즘에 내가 개발하면서 가장 중요시 하는 부분이 있다.  
자신이 사용하는 기술을 왜 사용하는지 논리적으로 설명할 수 있어야 한다고 생각한다.  

신입 개발자라면.. 현재의 나는 취업 시장에서의 경쟁력이라는 핑계로 기초 CS 지식을 간과하고 개발하지 않나. 스스로 질문해 볼 필요가 있다.  
그저 많은 라이브러리와 API를 사용하는 경험 했다면. 가까운 미래에 운 좋게 취업은 할 수 있을지 모르겠지만. 진짜 자신의 코딩 실력은 늘지 않고, 나중에는 그저 새로운 라이브러리를 공부하는데 허덕이는 자신을 마주할 것이라고 믿는다.

오늘 미디엄의 좋은 [포스팅](https://bit.ly/3haCw9e)을 보고 개념을 글로 정리하는 것을 실행에 옮기게 됐다.  
내가 과연 이 기술을 알고 쓰는 걸까 싶은 기술들을 앞으로 CS와 연관 지어서 포스팅해 보려고 한다.

## Nginx의 활용

"springboot 무중단 배포" 라고 구글링 하면 "Nginx를 통한 무중단 배포" 라는 키워드를 보지 않을 수 없다.  
[toss](https://toss.im/) `Response Headers` 를 보면 Server가 `nginx` 이다. (front cache server로 생각든다)

### Nginx 란?

Nginx에 대한 굉장히 자세한 내용은 나 포스트보다 [공식문서](https://www.nginx.com/resources/library/infographic-inside-nginx/)를 참고하는게 좋다.

Nginx 에서는 Nginx에 대해서 아래와 같이 설명한다.  

> 많은 web server와 application server는 단순한 쓰레드, 프로세스 기반 아키텍쳐 이지만 Nginx는 혁신적인 이벤트 중심 아키텍쳐이다.  
> 이를 통해 최신 하드웨어에서의 수십만의 동시 접속을 이뤄낸다고 한다.

## Nginx의 성능을 이해하려면 동작 방식을 알 필요가 있다.

Nginx의 동작방식을 이해하는데 필요한 개념

* [PCB & IPC & Context Switching](../CS/PCB-IPC-ContextSwitching.md)
* [I/O multiplexing](../CS/IO-Multiplexing.md)
* IRQ (Interrupt request lines)

## Nginx가 기존의 웹 서버(Apache)와 차이점

* Nginx가 개선한 것
  * 소개에도 강조했던 동시접속 (concurrent connections)
* 이를 위해 개선된 설계
  * Event-driven architecture
    * 연결 전담 프로세스나 스레드를 두지 않는다.
    * worker 프로세스들이 listen socket, connection socket 2개의 이벤트를 다 처리한다.
  * non-blocking I/O
* 설계를 구현한 방식
  * 고정된 수의 worker 프로세스만을 관리한다. 
    * 매 connection마다 프로세스나 스레드를 생성하지 않는다.
  * 프로세스를 이벤트를 기반으로 non-blocking 하게 동작한다.
  * 리소스나 Context Switching도 현저히 줄인다.

