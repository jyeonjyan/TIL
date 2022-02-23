# PCB & IPC & Context Switching

## Context Switching 역할에 질문 던지기

### 컴퓨터에서 하나의 Task만 처리할 수 있다면?

* 해당 Task가 끝날때까지 다음 Task는 기다릴 수 밖에 없다. (Blocking)
* 또한 반응속도가 매우 느리고 사용하기 불편하다.

### 그렇다면 사람들이 동시에 사용하는 것처럼 하기 위해서는?

* Computer multitasking을 통해 빠른 속도로 응답할 수 있다. 
* 빠른 속도로 Task를 바꿔가며 실행하기 때문에 실시간 처럼 보이게 되는 장점이 있다.
* CPU가 task 를 바꿔가며 실행하기 위해 Context Switching이 필요하게 되었다. 

## Context Switching
> 현재 진행하고 있는 Task의 상태를 저장하고 다음 진행할 Task의 상태값을 읽어 적용하는 과정을 말한다.

### Context Switching은 어떻게 진행될까?

* Task의 대부분 정보는 Register에 저장되고 PCB로 관리되고 있다. 
* 현재 실행하고 있는 Task의 PCB 정보를 저장하게 된다. (Process Stack, Ready Queue)
* 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행을 할 수 있다.

### Context Switching Cost

* Context Switching이 발생하면 많은 Cost가 소요됩니다.
  * cache 초기화
  * memory mapping 초기화
  * kernel은 항상 실행


### Process vs Thread

* Context Switching의 비용은 Process(PCB) > Thread(TCB)
  * Thread는 Stack영역을 제외한 모든 메모리를 공유한다.
  * Context Switching 발생시 Stack 영역만 변경하면 된다.
    * 장애 발생시 프로세스 하위의 쓰레드를 모두 종료해야 하는 단점을 보인다. (InternetExplorer)


## PCB, TCB

* PCB
  * 프로세스 상태와 정보를 저장하는 Block
* TCB
  * 쓰레드별로 존재하는 자료구조. `Program Counter`, `Register Set`, `PCB`를 가르키는 포인터를 갖는다.
* 데이터의 양: PCB > TCB  
  * <img src="../img/PCB-TCB.jpeg" width="500px"> 

## IPC

Process는 완전히 독립된 실행객체이기 때문에 별도 설비 없이는 프로세스간 통신이 어렵다는 문제가 있게 됩니다.  
이를 위해서 커널 영역에서 `IPC(Inter Process Communication)`을 제공하게 되고 프로세스는 커널이 제공하는 `IPC` 설비를 사용해서 프로세스간 통신을 할 수 있게 됩니다.

* PIPE (익명 PIPE)
  * 두 개의 프로세스를 연결하게 되고 하나의 프로세스는 데이터를 쓰기만, 다른 하나는 읽기만 할 수 있어요. (반이중) 통신
  * 하나의 프로세스가 단지 읽기만 하고 하나의 프로세스가 단지 쓰기만 하는 단순한 데이터 흐름이라면 PIPE 사용을 권장
* Named PIPE (FIFO)
  * 부모 프로세스와 무관하게 전혀 다른 프로세스 사이에서 통신이 가능
  * read-only, write-only만 가능하다.
* Message Queue
  * 메모리 공간, 파이프가 아닌 어디에서나 물건을 꺼낼 수 있는 컨테이너 벨트 형식
* Shared Memory
  * 데이터를 아예 공유하는 방법이다, 공유메모리가 데이터 자체를 공유하도록 지원하는 설비이다.
  * 중개자가 없어 IPC들 중에서 가장 빠르게 작동할 수 있다.
* Memory Map
  * 열린파일을 메모리에 맵핑시켜서 공유한다.
* Socket
  * 프로세스와 시스템의 기본적인 부분이며 프로세스들 사이에서 통신을 가능하게 한다
* Semaphore
  * 공유된 자원에 여러개의 프로세스가 동시에 접근하면 안되어서 하나의 프로세스만 접근 가능하도록 만들어준다.