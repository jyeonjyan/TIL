## thread 란
스레드(thread)란 프로세스 내에서 실제로 작업을 수행하는 주체를 의미한다.  
모든 프로세스에는 한 개 이상의 스레드가 존재하여 작업을 수행한다.  
또한, 두 개 이상의 스레드를 가지는 프로세스를 멀티스레드 프로세스라고 합니다.

## 스레드의 생성과 실행
자바에서 스레드를 생성하는 방법에는 다음과 같이 두 가지 방법이 있습니다.  
1. Runnable 인터페이스를 구현하는 방법
2. Thread 클래스를 상속받는 방법

두 방법 모두 스레드를 통해 작업하고 싶은 내용을 `run()` 메소드에 작성하면 됩니다.

```java
class ThreadWithClass extends Thread {
    public void run(){
        for (int i=0; i<5; i++){
            System.out.println(getName());
            try{
                Thread.sleep(10);
            } catch(InterruptedException e){
                e.printStackTrace();
            }
        }
    }
}
class ThreadWithRunnable implements Runnable {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
public class Thread01 {
    public static void main(String[] args){
        ThreadWithClass thread1 = new ThreadWithClass();       // Thread 클래스를 상속받는 방법
        Thread thread2 = new Thread(new ThreadWithRunnable()); // Runnable 인터페이스를 구현하는 방법
        thread1.start(); // 스레드의 실행
        thread2.start(); // 스레드의 실행
    }
}
```

실행결과
```
Thread-0
Thread-1
Thread-0
Thread-1
Thread-0
Thread-1
Thread-0
Thread-1
Thread-0
Thread-1
```

## 스레드의 우선순위
자바에서 각 스레드는 우선순위(priority)에 관한 자신만의 필드를 가지고 있습니다.  
이러한 우선순위에 따라 특정 스레드가 더 많은 시간 동안 작업을 할 수 있도록 설정할 수 있습니다.  

```java
static int MAX_PRIORITY: 스레드가 가질 수 있는 최대 우선순위를 명시함.
static int MIN_PRIORITY: 스레드가 가질 수 있는 최소 우선순위를 명시함.
static int NORM_PRIORITY: 스레드가 생성될 때 가지는 기본 우선순위를 명시함.
```
getPriority()와 setPriority() 메소드를 통해 스레드의 우선순위를 반환하거나 변경할 수 있습니다.  
스레드의 우선순위가 가질 수 있는 범위는 1부터 10까지이며, 숫자가 높을수록 우선순위 또한 높아집니다.

하지만 스레드의 우선순위는 비례적인 절댓값이 아닌 어디까지나 상대적인 값일 뿐입니다.  
**우선순위가 10인 스레드가 우선순위가 1인 스레드보다 10배 더 빨리 수행되는 것이 아닙니다.**  
단지 우선순위가 10인 스레드는 우선순위가 1인 스레드보다 좀 더 많이 실행 큐에 포함되어, 좀 더 많은 작업 시간을 할당받을 뿐입니다.
