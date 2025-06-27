# thread dump 를 통해 gradle build 실패 원인 분석

## TL;DR

* 최근에 회사에서 springboot 2.5 버전에서 3.4 버전으로 업그레이드 하는데 `./gradlew test` 에서 실패하는 문제를 발견 함.
* 나한테 보이는건 단순히 console 창에 `compileTestKotlin` step 에서 hang이 걸리고 있는 것이였음. 
* 근데 이 hang 이 왜 걸리는건지 알 수 없었고, 도대체 어떻게 문제 진단을 시작해야하는지 감이 잡히지 않았음.
* thread dump 를 이용해 어떤 thread 에서 hang 이 걸리는지 파악하고 문제를 진단하는데 도움을 받음.


## ./gradlew test 시에 compileTestKotlin 에서 hang 이 걸린다.

```kotlin
class CrashClass(
    private val injectClass: InjectClass
)

sealed class InjectClass()
data class InjectClassA(
    val value: String
) : InjectClass()
```

이런 Class 가 application 에 있다고 가정하고

```kotlin
@Test
fun contextLoads() {
    CrashClass(
        InjectClassB("test")
    )
}
```

이런 테스트코드가 존재했음. (여기서 `InjectClassB` 를 넣어주는 이유는 없는 클래스를 주입하는 상황을 재현하기 위함임)  
무슨 이유에서인지 IDE 에서 테스트코드에 compile error 를 잡아주지 않았고, 문제가 없겠지 하고 `./gradlew test` 를 수행 함.  

이때 `./gradlew test` 가 수행되다가 `compileTestKoltin` task 에서 다음 task 로 넘어가지 못하는 문제를 발견 함.  

## thread-dump 떠서 원인분석

당시에 compile error 가 왜 발생하는지 인지하지 못했음. 만약에 발생한다고 하더라도 해당 task 를 처리하다가 `FAILED` 로 종료되길 기대했음.  
무엇이 문제인지를 진단해야하는데, 어떻게 진단해야할지 감이 안잡혔는데, 태진님께서 thread-dump 를 떠봐라 라고 하셨음.  
아.. 이런 상황에서 thread-dump 를 떠보는것도 문제 원인을 파악하는데 도움이 되겠네 라는 생각이 들었음.

아래와 같이 다른 시간에 thread-dump 를 뜨고 지표의 변화를 관찰함.
```shell
// gradle test 를 돌린다.
./gradlew clean test 

// 실행된 gradle 데몬의 PID 를 확인한다.
ps aux | grep gradle

// gradle 데몬에 5초 간격으로 thread-dump 를 뜬다.
jstack -l {PID} > thread-dump-1.txt
sleep 5
jstack -l {PID} > thread-dump-2.txt
sleep 5
jstack -l {PID} > thread-dump-3.txt
```

3번의 thread-dump 에서 44번째 daemon thread 의 cpu time 이 선형적으로 증가하는걸 파악 함.

```
RMI TCP Connection(12)-127.0.0.1" #44 daemon prio=5 os_prio=31 cpu=154980.25ms elapsed=156.46s tid=0x0000000123a48e00 nid=0xdd03 runnable  [0x000000035440d000]
   java.lang.Thread.State: RUNNABLE
at org.jetbrains.kotlin.types.checker.ClassicTypeSystemContext$DefaultImpls.argumentsCount(ClassicTypeSystemContext.kt:193)
at org.jetbrains.kotlin.resolve.calls.components.ClassicTypeSystemContextForCS.argumentsCount(ClassicTypeSystemContextForCS.kt:23)
at org.jetbrains.kotlin.resolve.calls.inference.model.NewConstraintSystemImpl.argumentsCount(NewConstraintSystemImpl.kt)
```

> 이때 elapsed time 은 thread 가 생성된 후 시간이라서 cpu time (CPU 를 얼마나 사용했는지) 를 보는게 도움이 됨.

아무튼 kotlin compiler 쪽에서 type 리졸빙을 해서 넣어주는데, 존재하지 않은 type 을 넣어주려다 보니 리졸빙이 안되는것 같고, 따라서 "무한 루프(`java.lang.Thread.State: RUNNABLE` 상태 CPU time 증가)"가 돌고 있는것으로 추정해볼 수 있었음.

## 의심가는 테스트 삭제

의심가는 테스트코드를 삭제하고 돌려보니 `./gradlew test` 가 성공 함.  
**아무리 생각해봐도 클래스 리졸빙이 안되면 바로 실패하는게 맞는것 같은데, 내 코틀린 버전에서는 무한루프를 돌아서 이슈를 뒤져봤음.**

> https://youtrack.jetbrains.com/issue/KT-56815/compileKotlin-task-is-stuck-with-whiletrue-and-suspend-function

찾아보니 나와 같은 문제를 겪은 사람이 있었고, [Kotlin 1.8.20](https://github.com/JetBrains/kotlin/blob/master/docs/changelogs/ChangeLog-1.8.X.md#1822) 버전에서 해결된 것으로 보임. (이하 버전에서는 무한루프 재현)

## 마무리 

thread-dump 를 뜨는것은 어렵지 않음, 다만 thread-dump 를 떠서 분석해야겠다는 생각을 해보지 못했는데, 이번 일을 통해 배웠음.  
분석 기법을 일상적으로 사용하다보면 문제의 원인을 빨리 찾는데 도움이 될거라고 생각 함.  
분석 툴이 알려주는 정보가 해결 방법까지 제시하지 못하더라도, **이건 "문제"** 라고 인지할 수 있게 도와 줌.  
앞으로 문제가 발생했을때 메트릭을 뽑고 분석하는 습관을 길러야겠음.