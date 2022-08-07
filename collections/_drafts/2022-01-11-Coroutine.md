---
title: "Coroutine"
categories:
  - Android
  - Kotlin
tags:
  - Android
  - Coroutine
  - Flow
  - Kotlin
---

> Kotlin Coroutine 공식 가이드 : https://developer.android.com/kotlin/coroutines?hl=ko
> Coroutine의 정의 : https://ko.wikipedia.org/wiki/%EC%BD%94%EB%A3%A8%ED%8B%B4
# 코루틴 (Coroutine)
코루틴(Coroutine)은 [협동루틴](#https://ko.wikipedia.org/wiki/%EC%BD%94%EB%A3%A8%ED%8B%B4) 이라 부릅니다. 일반적으로 프로그래밍을 할 때에는 루틴이 실행되고 필요에 의해서 함수호출이 되면 서브루틴이 실행됩니다.
하지만 코루틴을 이용하면 루틴과 서브루틴의 관계가 아닌 서로가 서로를 호출하는 협동루틴으로 프로그램이 실행됩니다. 

또 다른 설명으로는 코루틴은 [경량스레드(Light-Weight-Thread)](#https://kotlinlang.org/docs/coroutines-basics.html#coroutines-are-light-weight) 라고도 불립니다. 
스레드 처럼 실행되지만 스레드와는 다릅니다. 코루틴은 스레드 안에서 실행되지만 스레드에 종속되지 않습니다. 즉, 하나의 스레드에서 출발하여, 실행되다가 멈춘(suspend) 뒤, 다시 재개(resume)할때, 다른 스레드에서 다시 시작될 수 있습니다.
하나의 코루틴에서 다른 코루틴이 실행될때 스레드처럼 Context Switching이 일어나지 않기때문에 리소스를 절약할 수 있습니다.

아래의 코드를 실행해서 차이를 비교해보겠습니다.

소스 참조 : https://www.charlezz.com/?p=44634

``` kotlin
fun coroutineTest() {
    runBlocking {
        println("시작 : 활성화 된 스레드 갯수 = ${Thread.activeCount()}")
        val time = measureTimeMillis {
            val jobs = ArrayList<Job>()
            repeat(100_000) {
                jobs += launch(Dispatchers.Default) {
                    delay(1000L)
                }
            }
            println("끝 : 활성화 된 스레드 갯수 = ${Thread.activeCount()}")
            jobs.forEach { it.join() }
        }
        println("처리시간 : $time ms")
    }
}

fun threadTest() {
    runBlocking {
        println("시작 : 활성화 된 스레드 갯수 = ${Thread.activeCount()}")
        val time = measureTimeMillis {
            val jobs = ArrayList<Thread>()
            repeat(100_000) {
                jobs += Thread {
                    Thread.sleep(1000L)
                }.also { it.start() }
            }
            println("끝 : 활성화 된 스레드 갯수 = ${Thread.activeCount()}")
            jobs.forEach { it.join() }
        }
        println("처리시간 : $time ms")
    }
}

fun main(args: Array<String>) {
    coroutineTest()
    Thread.sleep(1000)
    threadTest()
}
```
실행결과
```
시작 : 활성화 된 스레드 갯수 = 2
끝 : 활성화 된 스레드 갯수 = 19
처리시간 : 1387 ms
시작 : 활성화 된 스레드 갯수 = 18
끝 : 활성화 된 스레드 갯수 = 9178
처리시간 : 21258 ms
```

코루틴과 스레드의 차이는 개수가 많아질 수록 확연한 차이가 나는 것을 확인할 수 있다.





안드로이드 개발을 하다보면 안드로이드 장비만으로 서버-클라이언트 앱개발을 할 때가 있습니다. 이때 서버와 클라이언트 앱 모두 디버깅을 하면서 개발을 해야하는데, 이때 가상머신을 활용하면 편리합니다.

가상머신은 안드로이드 스튜디오에서도 제공하고 있지만, 가상 공유기 설정등을 활용하기 위해 다른 방법을 사용하겠습니다.

먼저 Genius





This theme supports **link posts**, made famous by John Gruber. To use, just add `link: http://url-you-want-linked` to the post's YAML front matter and you're done.

> And this is how a quote looks.

Some [link](#) can also be shown.

m-header-overlay-black-filter.jpg" | relative_url }})

```java
import System.IO;

public class App{
    public static void main(String[] args) {
        
    }
}
```

Or if you want to do more fancy things, go full rgba:
