---
title: Kotlin StateFlow와 SharedFlow 차이
author: jaepark
date: 2022-11-01 04:58:00 +0900
categories: [Android]
tags: [compose]
pin: false
img_path: '/assets/img'
---
>StateFlow와 SharedFlow의 차이점을 공부하기 전에 알아야하는 개념이 있었습니다. 그것은 바로 'Cold Stream'과 'Hot Stream'입니다.

## Cold Stream
- 하나의 소비자(Consumer)에게 값을 보냅니다.
- 생성된 이후에 누군가 소비하기 시작(collect)하면 데이터를 발행합니다.
- 상태가 변하지 않는 데이터를 읽을 때 Cold Stream의 데이터 형태가 적당 합니다.

## Hot Stream
- 하나 이상의 소비자(Consumer)에게 값을 보낼 수 있습니다.
- 데이터 발행이 시작된 이후 부터 모든 소비자에게 같은 데이터를 발행하고 구독자가 없는 경우에도 데이터를 발행합니다.
- 상태가 변하는 데이터를 읽을 때 Hot Stream의 데이터 형태가 적당 합니다.


Kotlin의 Flow는 기본적으로 Cold Stream 입니다. 즉, 구독 시점에 스트림이 생성되어 데이터를 처음부터 수신합니다. 
이를 Hot Stream으로 제공해주는 것이 StateFlow와 SharedFlow입니다. 두 개 모두 Hot Stream이라는 공통점이 있지만 명확한 차이점이 존재합니다.
이 두개의 차이점을 이해하기 위해서는 상태와 이벤트의 차이점을 알면 이해하기 쉽습니다.
- 상태(State)는 초기값이 있지만, 이벤트(Event)는 초기값이 없습니다. 로그인 상태에서 예를들면 초기값이 로그아웃인 반면, 
로그인 이벤트에서는 어떤 이벤트가 올지 모르기 때문에 초기 상태를 정의 할 수 없습니다.
- 상태(State)는 신규 구독 시 가장 최근 발행한 값을 받지만, 이벤트(Event)는 구독 이후 발생한 값을 받습니다.

이제 상태(State)와 이벤트(Event)의 차이를 기억하면서 StateFlow와 SharedFlow를 살펴 보겠습니다.

## StateFlow
- SharedFlow를 상속하고 있으며 선언 시 초기 값을 반드시 설정해야 합니다.
- StateFlow에서 collect를 하게 되면 가장 최근에 발행한 값을 받습니다.
- 이는 상태 값에 어울리는 데이터 형식이라 볼 수 있습니다.

## SharedFlow
- SharedFlow는 선언 시 초기 값을 선언하지 않아도 되며, 몇 가지 옵션을 전달하여 SharedFlow의 동작을 정의 할 수 있습니다. 
- 다음 예제는 SharedFlow를 선언하는 동시에 몇 가지 옵션을 설정하는 코드입니다.

```kotlin
private val _systemEvent: MutableSharedFlow<Unit> = 
    MutableSharedFlow(
      replay = 0, 
      extraBufferCapacity = 1, 
      onBufferOverflow = BufferOverflow.DROP_OLDEST
    )
```
- replay = 0 : 새로운 구독자에게 이전 이벤트를 전달하지 않음
- extraBufferCapacity = 1 : 추가 버퍼를 생성하여 emit 한 데이터가 버퍼에 유지 되도록 함
- onBufferOverflow = BufferOverflow.DROP.OLDEST : 버퍼가 가득찼을 시 오래된 데이터 제거

replay 값을 0으로 설정 한다면, SharedFlow를 구독 한 이후 데이터 스트림을 받을 수 있게됩니다. 
즉, collect를 하게 된 시점부터 발행 된 데이터를 받게됩니다. 이는 Event의 특성과 매우 어울리는 형식이라고 할 수 있습니다.

위와 같은 특성 때문에 StateFlow는 상태 데이터, SharedFlow는 이벤트 데이터를 받을 때 사용하는 것이 적합하다고 볼 수 있습니다.

**참고**  
[Kotlin 공식 문서](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow)  
