---
title: Android Activity LifeCycle에 대해 정리
author: jaepark
date: 2020-10-06 14:48:00 +0900
categories: [Android]
tags: [activity lifecycle]
pin: false
img_path: '/assets/img/android'
---
> Android 개발을 할 때 LifeCycle을 모르면 안될 정도로 중요 합니다. 만약 LifeCycle을 모르고 무작정 개발을 시작했다면, 
> 오류가 났을 때 무엇이 원인 인지 찾을 수 없을지도 모릅니다. 대학 시절 교수님이 제일 중요하게 강조 하셨던 
> 그 Lifecycle을 공부하며 정리 해볼까 합니다.

## **Activity의 Lifecycle이란?**

안드로이드 공식 블로그에서는 따로 사전적 정의는 없습니다만, lifecycle을 activity의 현재 상태를 콜백해주는 메소드의 집합으로 소개 하고 있습니다. 
activity 상황에 따라 어떤 메소드들이 콜백 하는지 이해 한다면, 동영상 플레이어를 사용자가 다른 앱을 켰을 때 정지하고, 
또 다시 앱을 구동 시켰을 때 재생 시키며, 앱을 구동하고 있는 상태에서 갑자기 전화 올 때 등등 여러 시나리오에서 대처 할 수 있습니다.

다음은 그 유명한 lifecycle 호출 순서에 대한 이미지 입니다.

![activity_lifecycle.png](..%2F..%2F..%2Fassets%2Fimg%2Fandroid%2Factivity_lifecycle.png)

## **onCreate()**
시스템이 activity를 처음 만들 때 호출되는 메서드입니다. 주로 xml을 바인딩 하거나 set 하는 코드를 여기서 작성합니다. 
즉, activity가 사용자에게 보이기 전 최초로 만들어야 하는 모든 과정을 onCreate() 메서드 안에서 만든다고 볼 수 있습니다.

## **onStart()**
onCreate() 메서드 호출이 끝나면 다음으로 onStart() 메서드가 호출됩니다. onStart() 메서드가 호출됐을 때는, 
activity가 유저에게 보이도록 동작합니다. 아직 UI가 보이지 않으며, 이를 가능하게 만들기 위해 앱을 포그라운드에 놓이도록 준비해 주는 역할을 합니다.

## **onResume()**
onStart() 메서드의 호출이 끝나면 바로 onResume() 메서드가 호출됩니다. onResume() 상태가 됐다면, 
activity는 포그라운드에 놓이게 되며, 화면이 사용자에게 보임으로 사용자와 앱 UI 간의 상호작용이 가능해집니다. 
특별한 이벤트가 없는 이상 activity는 onResume() 상태에 머무르게 됩니다.

## **onPause()**
이 메서드가 실행됐다는 것은 더 이상 activity는 포그라운드에 놓이지 않다는 뜻입니다. 
그렇게 되면 유저는 activity와 상호작용을 할 수 없지만, 아직까지는 activity는 화면에 보입니다.

activity가 onPause 상태에 진입하는 경우는 다음과 같습니다.
- 어떠한 이벤트가 앱에 exception을 일으키는 경우.
- Android 7.0 (API 24) 이상에서, 앱이 멀티윈도우 모드로 전환되는 경우 (onPause 상태에 있다가 멀티뷰로 볼 새로운 app이 화면에 띄워질 경우 onResume() 상태로 전환됩니다.)
- 다이얼로그 같은 화면이 띄워짐으로, activity가 포커스가 되지 않고 부분적으로 보이게 됐을 경우

## **onStop()**
acitivity가 onStop()에 놓이게 되는 경우, activity는 더 이상 유저에게 보이지 않습니다. onStop() 메서드가 호출되는 시기는 새로운 activity가 유저에게 보이고 기존에 있던 activity를 가렸을 경우입니다. 

## **onDestroy()**
onDestroy() 메서드는 activity가 완전히 destroyed 되기 전에 호출이 됩니다. onDestroy가 호출되는 시기는 다음의 두 가지 이유 때문입니다.
- activity가 완전히 끝났을 때 ( finish() 함수가 호출되거나, 유저가 activity를 완전히 사라지게 했을 때)
- 화면 회전 및 멀티뷰 전환같이 시스템이 일시적으로 activity를 destoryed 했을 때

## **startAcitivity의 Lifecycle**
lifecycle 관련 공부하다가 박상권님의 블로그에서 유용한 내용이 있어서 기록에 남깁니다. 다음과 같은 질문을 받을 때 대부분의 개발자가 틀린 답을 말했다 합니다.
> Main Activity에서 Detail Activity를 호출했을 때 라이프사이클이 호출되는 순서를 나열해보세요

정답은 다음과 같습니다.
- [Main]onPause()
- [Detail]onCreate()
- [Detail]onStart()
- [Detail]onResume()
- [Main]onStop()

반대의 경우를 살펴 보겠습니다. 
> Detail Activity 종료후 다시 Main Activity가 보여질 때 라이프사이클이 호출되는 순서를 나열해보세요

정답은 다음과 같습니다.
- [Detail]onPause()
- [Main]onRestart()
- [Main]onStart()
- [Main]onResume()
- [Detail]onStop()
- [Detail]onDestroy()

**참고**  
- [Android Developer](https://developer.android.com/guide/components/activities/activity-lifecycle)
- [지원자 95%가 틀리는 startActivity()라이프사이클, 당신도 예외는 아닙니다](https://medium.com/%EB%B0%95%EC%83%81%EA%B6%8C%EC%9D%98-%EC%82%BD%EC%A7%88%EB%B8%94%EB%A1%9C%EA%B7%B8/%EC%A7%80%EC%9B%90%EC%9E%90-95-%EA%B0%80-%ED%8B%80%EB%A6%AC%EB%8A%94-startactivity-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EB%8B%B9%EC%8B%A0%EB%8F%84-%EC%98%88%EC%99%B8%EB%8A%94-%EC%95%84%EB%8B%99%EB%8B%88%EB%8B%A4-ed0947a48d6)
