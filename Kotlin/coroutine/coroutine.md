# Coroutine
> https://kotlinlang.org/docs/coroutines-guide.html

> https://speakerdeck.com/taehwandev/kotlin-coroutines

> https://www.youtube.com/watch?v=eJF60hcz3EU



# Kotlin in Action 내용 정리
## 정의
- 코루틴은 컴퓨터 프로그램 구성 요소 중 하나로 비선점형 **멀티태스킹**(non-preemptive multitasking)을 수행하는 일반화한 **subroutine**이다.
- 즉 실행을 일시중단(suspend)하고 재개(resume)할 수 있는 여러 진입 지점(entry point)를 허용한다.
  
### 서브루틴이란?
- 정의) 여러 명령어를 모아 이름을 부여해서 반복 호출할 수 있게 정의한 프로그램 구성요소
- 동의어) 함수
- 진입하는 방법은 한 가지뿐이며(함수 호출)
- 진입할 때마다 활성 레코드(activation record)가 스택에 생김
- 진입과 반대로 함수를 나가는, 즉 return 값을 돌려주는 지점은 여러 가지가 될 수 있다.
  - = return 조건에 따라 여러 개 적을 수 있단 이야기

### 멀티태스킹이란?
- 여러 작업을 동시에 수행하거나 그렇게 사용자가 보이도록 하는 것
- 비선점형이란?
  - 멀티태스킹의 각 작업을 수행하는 참여자들의 실행을 OS가 강제로 일시 중단시키고 다른 참여자를 실행하게 만들 수 없는 것
  - 즉 multitasking에 대한 scheduling에서 OS(외부요인)로부터 독립적인 것
  - 즉 참여자들이 직접 자발적으로 협력하여 multitasking을 수행하는 것
  
### 결론
- **"서로 협력해서 실행을 주고 받으면서 작동하는 여러 서브루틴(함수 실행)"**

## Generator(대표적인 코루틴)의 제어 흐름
- 함수 A, B가 있다고 할 때
- A가 실행되다가 (ㄱ)지점에서 제너레이터(코루틴) B를 호출하면
- 동일한 스레드에서 B가 실행된다.
- B가 실행되다가 yield 키워드를 통해 다시 A로 돌아간다.
- A는 다시 (ㄱ)지점 이후 코드를 실행하고 다시 (ㄴ)에서 B를 호출한다.
- B는 yield가 있는 이후 코드부터 실행된다.

> 위 실행 흐름에서 원래는 return 과 비슷한 개념으로 본다면 yield 이후 B의 지역변수 등에 대한 정보는 모두 없어져야 맞지만, coroutine(generator)를 사용하면 그렇지 않다.

## 코루틴 장점
- 위의 제어흐름이 갖는 장점이 뭐가 있을까?
1) 일반적인 프로그램 로직을 기술하듯 코드를 작성하고 상대편 코루틴에 데이터를 넘겨야 하는 부분에서만 yield 키워드를 사용하면 된다.


### 1)에 대한 예시
> 카운트 다운(10, 9, ..)를 출력하는 기능을 구현하라
- generator 활용
~~~
generator countdown(n){
    while(n > 0){
        yield n
        n -= 1
    }
}

for i in countdown(10){
    println(i)
}
~~~

## 코루틴 설정
- 코틀린에서의 코루틴 기능은 언어가 지원하는 형태가 아니라 
코루틴을 구현할 수 있는 기본 도구를 언어가 제공하는 형태이기 때문에 
kotlin.coroutine 패키지 하위에 있다.
  - https://github.com/Kotlin/kotlinx.coroutines
### maven 설정
~~~
<dependency>
    <groupId>org.jetbrains.kotlinx</groupId>
    <artifactId>kotlinx-coroutines-core</artifactId>
    <version>1.0.1</version>
</dependency>
~~~

### gradle
~~~
dependencies{
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.0.1")
}
~~~

### android
~~~
implementation 'rg.jetbrains.kotlinx:kotlinx-coroutines-android:1.0.1'
~~~

## Corutine builder(core)
- 코틀린에선 주로 builder에 원하는 동작을 람다로 넘겨서 코루틴을 만들어 실행한다.

### Launch(kotlinx.coroutines.CoroutineScope.launch)
- 코루틴을 job으로 변환하여 기본적으로 즉시 실행된다.
- launch가 반환한 job의 cancel()을 호출해 코루틴 실행을 중단시킬 수도 있다.
- 작동하려면 CoroutineScope 객체가 this로 지정되어야 한다.
  - 호출한 함수가 사용 중인 게 있다면 해당 CoroutineScope가 되겠지만
  - 없는 경우 GlobalScope를 사용하면 된다.
~~~
import kotlinx.coroutines.*
import java.time.ZonedDateTime

fun now()=ZonedDateTime.now().toLocalTime().truncatedTo(ChronoUnit.MILLIS)
fun log(msg: String) = println("${now()}:${Thread.currentThread()}: ${msg}")

fun launchInGlobalScope(){
    GlobalScope.launch {
        log("coroutine started")
    }
}

fun main(){
    log("main started")
    launchInGlobalScope()
    log("launchInGlobalScope() executed")
    Thread.sleep(5000L)
    log("main() terminated")
}
~~~

- 이를 실행한 결과를 보면, 다음과 같다
~~~
main started
launchInGlobalScope executed
launchInGlobalScope coroutine started
main terminated
~~~ 
- 만약 Thread.sleep(5000L)이 없다면 launch가 아예 실행되지 않았을 것이다.
  - GlobalScpoe는 메인 스레그가 실행 중일 때만 동작을 보장하기 떄문.
  - 이를 방지하기 위해 비동기적으로 launch를 실행하거나, launch가 모두 다 실행될 때까지 기다려야 한다.
    - 코루틴 실행이 끝날 때까지 현재 스레드를 블록하는 runBlocking() 함수

### sync, async, block, non block
> https://jh-7.tistory.com/25

- IBM developerWorks의 2:2 매트릭스
||Blocking|Non-Blocking|
|---|---|---|
|Synchronous|Read/Write|Read/Write(O_NONBLOCK)|
|Asynchronous|i/o multiplexing(select/poll)|AIO|

- Blocking / Non-blocking 은 호출된 함수가 호출한 함수에게 제어권을 바로 주느냐 안주느냐,
- Sync / Async 는 호출된 함수의 종료를 호출한 함수가 처리하느냐, 호출된 함수가 처리하느냐의 차이다.

- 블로킹 Blocking
  - A 함수가 B 함수를 호출 할 때, B 함수가 자신의 작업이 종료되기 전까지 A 함수에게 제어권을 돌려주지 않는 것
- 논블로킹 Non-blocking
  - A 함수가 B 함수를 호출 할 때, B 함수가 제어권을 바로 A 함수에게 넘겨주면서, A 함수가 다른 일을 할 수 있도록 하는 것.
- 동기 Synchronous
  - A 함수가 B 함수를 호출 할 때, B 함수의 결과를 A 함수가 처리하는 것.
- 비동기 Asynchronous
  - A 함수가 B 함수를 호출 할 때, B 함수의 결과를 B 함수가 처리하는 것. (callback)

-  IO(입출력)를 처리할 때 Sync와 Async의 차이는 그것을 요청한 순서가 지켜지는가 아닌가에 있고 Blocking와 Non-blocking은 그 요청에 대해 받은 쪽에서 처리가 끝나기 전에 리턴해주는가 아닌가에 달려 있습니다.
- 그래서 3개를 요청했는데 응답에서 그 순서가 지켜진다면 Sync이고 어떤 게 먼저 올지 모른다면 Async입니다.

### async
- launch와 차이 점
  - launch는 Job을 반환
  - async는 Deffered를 반환
- Job vs Deffered
  - Job을 상속받은 게 Deffered
  - Job은 타입 파라미터가 없는데
  - Deffered는 제너릭 타입
    - 코루틴이 계산을 하고 돌려주는 값의 타입 !
  - Deffered는 await()함수가 정의되어 있다.
- 따라서 코드 블록을 비동기 실행할 수 있고
  - 제공하는 코루틴 컨텍스트에 따라 여러 스레드를 사용하거나 한 스레드 안에서 제어만 왔다갔다할 수도 있다.
- async가 반환하는 Deffered의 await()를 이용하여 코루틴이 결과를 내놓기까지 기다렸다가 결과값을 계산할 수도 있다.

### async 중요 예제
- 강점
  - 실행하려는 작업은 시간이 짧게 걸리는 데에 반해
  - IO의 대기 시간이 크고
  - CPU 코어 수가 작아 동시에 실행 가능한 스레드 개수가 한정된 경우
  - 코루틴의 강점이 발휘된다.
- 코드
~~~
fun sumAll(){
    runBlocking{
        val d1 = async{delay(1000L); 1}
        log("after aync(d1)")
        val d1 = async{delay(1000L); 2}
        log("after aync(d2)")
        val d1 = async{delay(1000L); 3}
        log("after aync(d3)")

        log("1+2+3=${d1.await() + d2.await() + d3.await()}")
        log("after await all & add")
    }
}
~~~
- 결과값
~~~
00:46:45.405:Thread(main):after async(d1)
00:46:45.409:Thread(main):after async(d2)
00:46:45.409:Thread(main):after async(d3)
00:46:48.417:Thread(main):1+2+3=6
00:46:48.417:Thread(main):after await all & add
~~~
- 정리
  - d1, d2, d3가 '순서대로' 실행하면 6초가 걸려야 하지만
  - 절반만 걸린다.
  - 또한 async 코드로 실행하는 데에는 시간이 거의 걸리지 않았다.
  - 그럼에도 모든 aync함수는 메인스레드 안에서만 실행되었다.

### 코루틴 컨텍스트와 디스패처

### 코루틴 빌더와 일시중단 함수
- 코루틴 빌더 : launch, async, runBlocking, produce, actor
- 중단 함수 : delay(), yield(), withContext, withTimeout, withTimeoutOrNull, awaitAll, joinAll

### suspend()키워드와 코틀린의 일시중단함수 컴파일 방법
- 코루틴 외의 일반 함수에서 중단함수를 쓰면 어떻게 될까?
- 금지됨
- 어떻게 컴파일되길래?

### 직접 빌더 만들기