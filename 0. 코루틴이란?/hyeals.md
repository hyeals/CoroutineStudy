# 0. 코루틴이란?

## 0.1. 코루틴 정의
- **Co(Cooperation) + Routines(functions)** ⇒ *서로 협력하는 함수들이라는 의미*
    - 여러 함수들이 번갈아가면서 실행되어 비동기적 프로그래밍이 가능하도록 만듦

## 0.2.Light-Weight Thread라고 소개하는 이유는? 
1. 코루틴은 자체적으로 **스택을 갖고 있지 않음(stackless)** ⇒ native thread에 1:1로 매핑되지 않음
2. thread를 번갈아가면서 수행(Concurrency)될 때 사용되는 비용인 *context switching이 코루틴에서는 발생하지 않음* ⇒ 하나의 thread에 여러 개의 코루틴이 존재하며 
해당 **코루틴들이 서로 협력적으로 번갈아가면서 실행되기 때문**

- **thread**: native thread를 만들어야하기 때문에 *생성 개수가 제한적* (때문에 아래 코드에서 100000개의 thread를 만들 수 없음)
```kotlin
fun main() {
    repeat(100_000){
        Thread{
            Thread.sleep(5000L)
            print(".")
        }
    }
}

//print
출력 X
```

- **coroutines**: 하나의 thread에 여러 개의 코루틴 생성 가능
```kotlin
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch
import kotlinx.coroutines.runBlocking

fun main() = runBlocking{
    repeat(100_000){
        launch {
            delay(5000L)
            print(".")
        }
    }
}

// print
................................... ... . 
```
⇒ 서로 다른 100000개의 코루틴을 실행하여 각각 5초를 기다린 후에 매우 적은 메모리를 소비하면서 . 을 출력
