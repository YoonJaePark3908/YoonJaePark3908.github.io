---
title: Android Component란 무엇인가?
author: jaepark
date: 2022-02-25 12:03:00 +0900
categories: [Android, Tech]
tags: [android component]
pin: true
img_path: '/assets/img'
---
> Android Developer 홈페이지의 공식 문서를 읽다보면 Component에 대한 언급이 자주 등장 합니다.
> 그 만큼 android에서 기초가 되고 중요한 요소들이라는 것을 알 수 있는데요, Android Component에 대한 공부를 하며 내용을 정리 해볼까 합니다.

## **Component 정의**
컴포넌트는 앱의 구성 단위이며, 컴포넌트 여러 개를 조합하여 하나의 앱을 만듭니다. 
Android의 기초가 되는 Component의 구성 요소는 Activity, Service, Broadcast receiver, Content provider 총 4개가 있습니다. 

## **Component의 구성 요소**

### Activity(액티비티)
사용자와 상호작용하는 접점 포인트 입니다. 한 개의 화면에서는 한 개의 액티비티 만 포함돼 있어야 합니다.

### Service(서비스)
특정 동작을 background에서 지속적으로 수행 하도록 합니다. 예를들어, 앱이 띄워지지 않아도 음악을 재생 시키고 싶을 때 서비스를 사용하여 구현합니다.
보통 bacground에서 수행한다 하면, background thread에서 동작한다고 오해 할 수 있지만 사실 서비스 자체는 main thread(UI thread)에서 동작하기 때문에 ANR에 주의해야 합니다.

### Broadcast receiver(브로드캐스트 리시버)
애플리케이션 외부에서 받은 특정 이벤트를 처리합니다. 예를들어 알람 애플리케이션 같은 경우, 사용자가 특정 시간에 알람을 설정한 후 설정한 시간이 다가오면 
알람 애플리케이션은 브로드캐스트 리시버를 통해 이벤트를 받아 알람을 울리게 됩니다.

### Content provider(콘텐츠 프로바이더)
애플리케이션 간의 데이터 공유를 관리합니다. 예를들어, A 애플리케이션의 데이터베이스를 B 애플리케이션에 공유 하고 싶을 때 
database read, write에 대한 접근 권한 허용을 manifests 파일에 작성을 하고, 콘텐츠 프로바이더를 사용하여 구현합니다.

**참고**  
[Android Developer](https://developer.android.com/guide/components/fundamentals#Components)
