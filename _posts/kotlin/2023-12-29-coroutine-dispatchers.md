---
title: Kotlin Coroutine dispatchers 정리
author: jaepark
date: 2023-12-29 17:23:00 +0900
categories: [Kotlin]
tags: [coroutine]
pin: false
img_path: '/assets/img'
---
## **Dispacher란?**
코루틴이 어느 스레드 풀에서 작동할지 정해주는 역할을 합니다. Dispatcher의 종류는 Main, IO, Default, Unconfined 총 4가지가 있습니다.
> 스레드 풀 : 스레드를 미리 생성하고, 작업 요청이 발생할 때 마다 미리 생성된 스레드로 해당 작업을 처리하는 방식
{: .prompt-tip }

## **Main**
Android 메인 스레드에서 코루틴을 실행하는 디스패처입니다. UI와 상호 작용하는 작업을 위해서 사용합니다.

## **IO**
디스크 또는 네트워크 I/O 작업을 실행하는데 최적화 되어있는 디스패처입니다. Dispachers.IO를 설정해주면 백그라운드 스레드에서 실행이 됩니다. 
기본으로 설정된 스레드 풀의 스레드의 개수는 64개입니다.

## **Default**
Default 디스패처는 메인 스레드 외부에서 CPU를 많이 사용하는 일을 수행하는 데에 최적화되어있습니다. 
주로 직렬화, 역직렬화하는 json 파싱 작업, list sort 작업이 대표적입니다. 스레드 풀의 스레드 개수는 CPU 개수만큼 설정되어 있습니다.

## **Unconfined**
Unconfined 디스패처는 특정 스레드를 한정하지 않습니다. Coroutine Dispatcher인 Dispatchers. Unconfined는 처음 일시 중단 되기 전까지만 호출 스레드에서 Coroutine을 시작합니다. 
일시 중단 이후에는 실행된 일시 중단 함수에 의해 완전히 결정된 스레드 상에서 Coroutine을 재개합니다. 글로 이해하기 힘드니 Android Framework에서 예시 코드를 보겠습니다.
```kotlin
lifecycleScope.launch {
  launch(Dispatchers.Unconfined) { // not confined -- will work with main thread
    Log.d("Unconfined Dispatchers Test","Unconfined      : I'm working in thread ${Thread.currentThread().name}")
    withContext(Dispatchers.IO) {
      delay(500)
      Log.d("Unconfined Dispatchers Test","Unconfined      : After delay in thread ${Thread.currentThread().name}")
    }
  }
  launch { // context of the parent, main runBlocking coroutine
    Log.d("Unconfined Dispatchers Test","main runBlocking: I'm working in thread ${Thread.currentThread().name}")
    delay(1000)
    Log.d("Unconfined Dispatchers Test","main runBlocking: After delay in thread ${Thread.currentThread().name}")
  }
}
```
위 코드는 다음과 같이 출력이 됩니다.
```console
Unconfined      : I'm working in thread main
main runBlocking: I'm working in thread main
Unconfined      : After delay in thread DefaultDispatcher-worker-1
main runBlocking: After delay in thread main
```

처음 Unconfined를 실행했을 때 처음 호출된 main 스레드에서 돌아가지만 IO Dispatcher 안에 돌아가는 suspend 키워드의 delay 코드를 만나게 된 후 IO thread 풀에서 돌아가는 것을 확인할 수 있습니다. 
만약 IO Dispatcher를 설정하지 않고 그냥 돌리면 Default Dispatcher에서 돌아가는 것을 확인할 수 있습니다.

>[공식 문서](https://kotlinlang.org/docs/coroutine-context-and-dispatchers.html#unconfined-vs-confined-dispatcher)에서는 
> Unconfined Dispathcer의 경우 특이 케이스에서 도움이 되는 고급 매커니즘이기 때문에 일반 코드에서는 사용을 지양하라고 합니다.
{: .prompt-warning }

**참고**<br>
[Kotlin 공식 문서](https://kotlinlang.org/docs/coroutine-context-and-dispatchers.html)
[Coroutine의 Dispatcher란 무엇인가?](https://kotlinworld.com/141)<br>
