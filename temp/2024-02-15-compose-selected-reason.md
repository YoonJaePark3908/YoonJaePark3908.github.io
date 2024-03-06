---
title: Android 프로젝트에서 Compose UI를 선택한 이유
author: jaepark
date: 2024-02-15 10:38:00 +0900
categories: [Android]
tags: [compose]
pin: false
img_path: '/assets/img'
---

## 특정 트리거로 인해 UI Update 시 불필요한 렌더링 완화로 인한 성능 향상
[기존 View System에서 자식뷰가 새로 그려지게 되면 그 위에있는 ViewGroup 안에 있는 뷰도 같이 새로 그려지게 됩니다.](https://developer.android.com/topic/performance/rendering/optimizing-view-hierarchies#managing) 
이를 보완하기 위해 Google Android 팀은 ViewGroup의 Depth를 최소하기 위해 ConstraintLayout을 사용을 권장합니다.
**하지만 ConstraintLayout을 사용해도 depth는 줄지만, ConstraintLayout안에 있는 다른 View 들도 다시 렌더링이 된다는 뜻입니다.**
Compose는 이를 보완하여 Recompose 시 다시 그릴 필요없는 UI 렌더링을 스킵하도록 설계 되어있어 성능 향상를 불러옵니다.
그렇다면 Compose는 어떻게 불필요한 UI 렌더링을 스킵 할까요? 자세한건 여기 [링크](https://developer.android.com/jetpack/compose/lifecycle#skipping)에서 확인하실 수 있습니다.

## 개발 퍼포먼스 및 유지 보수 효율성 향상
Compose는 기존 xml과 비교 했을 때 개발 퍼포먼스 및 유지 보수 효율성이 다음과 같은 이유로 향상됩니다.

### 더 짧은 코드 작성
네이티브 코드와 xml 창을 동시에 띄우지 않아도 코드 하나로 개발이 가능

### 복잡한 상속 관계 해소
https://wooooooak.github.io/jetpack%20compose/2021/05/18/%EC%BB%B4%ED%8F%AC%EC%A6%88%EA%B0%80%ED%95%84%EC%9A%94%ED%95%9C%EC%9D%B4%EC%9C%A0/

### 위젯 분리의 편리함
위젯을 분리함으로써 코드의 재사용성이 늘어남

## Compose Multiplatform UI Framework 확장성에 대한 기대
https://www.jetbrains.com/lp/compose-multiplatform/
아직 iOS는 Alpha, 정식 출시 기대
