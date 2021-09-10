## springboot @Async 는 어떻게 동작하는가?
> [도움을 주셔서 감사합니다 🙇🏻‍♂️](https://brunch.co.kr/@springboot/401)  
> 스프링 프레임워크에서 제공하는 @Async 비동기 메서드 사용 방법에 대해서 알아보자

### 1. 병렬 프로그래밍

#### 비동기 vs 논블록킹
'비동기'와 '논블록킹'에 대한 비교는 개발자마다 조금씩 기준이 다른 것 같다.  
'비동기'는 메서드를 제공하는 입장에서의 개념이고, '논블록킹'은 메서드를 사용하는 클라이언트 입장에서의 개념이라고 생각한다.  
사실 두 개의 입장이 다르기 때문에 비교 대상이 아니라는 얘기다.

**비동기 & 논블록킹에 대해서 명확하게 정의할 수 있는 개발자는 필자에게 조언을 꼭 해주길 부탁한다.**

#### 쓰레드풀
순차 프로그래밍 예시를 먼저 알아본다. 수행시간이 1초가 걸리는 메서드가 있다고 가정한다.
```java
private static void task(){
    try{
        Thread.sleep(1000);
    } catch(InterruptedException e){
        e.printStackTrace();
    }
}
```

해당 메서드를 100번 실행하면?  
100초의 시간이 걸릴 것이다. 단 하나의 쓰레드로 모든 작업을 순차적으로 처리하게 된다.  
그래서 1초를 * 100 번 수행하기 떄문에 100초의 지연시간이 발생한다.

#### 병렬 프로그래밍으로 개선
짧은 시간에 모든 작업이 완료될 수 있도록 '병렬 프로그래밍'으로 개선해보자. `Executors.newFixedThreadPool` 메서드를 사용해서 쓰레드풀을 정의한다.

```java
private static final int THREAD_POOL_SIZE = 100;
private static final Executor executor = Executors.newFixedThreadPool(THREAD_POOL_SIZE);
```

