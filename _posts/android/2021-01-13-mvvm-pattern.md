---
title: Android MVVM 패턴
author: jaepark
date: 2021-01-13 17:47:00 +0900
categories: [Android]
tags: [mvvm]
pin: false
img_path: '/assets/img'
---
## **디자인 패턴의 필요성**
저는, 한 프로젝트에서 신규 기능 개발을 담당한 적이 있습니다. 이미 짜놓은 코드에서는 모든 데이터 및 네트워크 처리, UI 구성을 activity에 작성을 해둔 상태입니다. 
그 결과 activity 코드의 길이는 무려 4000줄이 넘었고, 코드가 너무 복잡한 상태가 이르러 사소한 에러 조차도 잡기에 시간이 오래 걸렸습니다. 
이 상태에서 디자인 패턴을 적용하지 않으면 activity의 코드 길이는 더 길어질 것이며, 돌이킬 수 없는 상황이 올 수 있습니다.
하지만 디자인 패턴을 적용하게 되면 에러난 부분을 쉽게 파악할 수 있으며, 코드의 수정, 추가 등등 유지 보수 측면에서 정말 유용합니다.

## **MVVM 패턴이란?**
Model, View, ViewModel의 약자로, android 뿐만 아니라 다양한 플랫폼에서 쓰이는 디자인 패턴 개념입니다. 각각의 역할은 다음과 같습니다.

![image_3](/android/mvvm/image_1.png)<br>

## **View**
- UI 요소들을 표현하며, 사용자가 발생한 이벤트를 받습니다.
- android에서는 주로 activity, fragment에서 이 기능을 담당합니다.

## **ViewModel**
- view에 표시 할 데이터를 관리합니다.
- view에서 요청 받은 데이터들을 model에서 가져와 가공하여, 데이터가 변경 됐다는 노티를 보냅니다. 
이 때 android에서 유용하게 사용하는 것이 LiveData의 setValue or postValue, Flow의 emit 같이 값이 변경 됐을 때 노티가 가능한 데이터 형식입니다.

## **Model**
- 비즈니스 모델에 적합한 데이터들을 처리합니다.
- android에서는 주로 repository에서 이 기능을 담당합니다. 

## **Android JetPack의 ViewModel 라이브러리**
Android에서는 MVVM의 ViewModel의 개념을 본따 만든 JetPack의 [ViewModel 라이브러리](https://developer.android.com/topic/libraries/architecture/viewmodel)가 있습니다.
Android의 ViewModel은 activity의 lifecycle을 따라갑니다. 즉, activity가 destroy가 됐다면, ViewModel 또한 소멸됩니다.
앱의 화면이 회전 됐을 때 ViewModel은 소멸하지 않아 데이터를 유지할 수 있습니다.
아래 그림은 ViewModel이 Activity의 생명주기를 나타내는 그림입니다. Activity가 finished 되면 ViewModel 또한 소멸 되는 것을 확인 할 수 있습니다.
![image_3](/android/mvvm/image_2.png)<br>

이와 같은 특징으로, ViewModel은 MVVM 패턴을 구현할 때 아주 유용하게 사용됩니다. MVVM 패턴을 사용할 때 ViewModel 라이브러리를 사용하지 않아도 되지만, 
구글이 아주 친절하게 MVVM 패턴과 android의 lifecycle을 고려한 ViewModel라이브러리를 만들어줬으니, 사용하지 않을 수 없습니다. 굳이 MVVM 패턴이 아니라도, 
다양한 방면으로 사용될 수 있는 아주 좋은 라이브러리 입니다.


하지만, MVVM의 ViewModel과 Android의 ViewModel은 다릅니다. MVVM의 ViewModel 개념은 android 외에도, 다양한 플랫폼에서 사용 되는 포괄적인 개념이며, 
Android의 ViewModel은 android의 lifecycle까지 고려하여 보다 더 빠르고 정확하게 MVVM 패턴을 기반으로 개발을 할 수 있도록 도우는 jetpack 라이브러리 입니다.
