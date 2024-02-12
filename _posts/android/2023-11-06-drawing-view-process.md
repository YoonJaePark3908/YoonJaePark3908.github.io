---
title: Android View가 그려지는 과정과 성능 최적화
author: jaepark
date: 2020-11-23 14:39:00 +0900
categories: [Android]
tags: [view]
pin: false
img_path: '/assets/img'
---
>글을 읽기전에 알아두면 좋은 개념<br>
>- LinearLayout, ConstraintLayout의 부모 Class : ViewGroup<br>
>- ImageView, TextView의 부모 Class : View
{: .prompt-tip }
## **View가 그려지는 순서**
View의 Lifecycle을 나타내는 자료들도 많지만, 본문의 목적은 UI가 그려지는 과정을 정리하기 위함으로 생략하겠습니다.
xml에서 흔히 사용하는 ImageView, TextView의 부모 클래스인 View Class가 그려지는 순서는 다음과 같습니다.

>Measure -> Layout -> Draw
{: .prompt-info }
### Measure
View Class 내부에 있는 public final void measure 함수가 실행 되고, 뷰 트리의 탑 다운으로 순회하면서 가로, 세로 길이를 측정합니다. 
이 때 모든 View는 부모 View의 제약 조건에 맞춰 가로, 세로 길이를 측정합니다. 

### Layout
layout 단계는 requestLayout() 메서드가 불러오면서 시작 됩니다. 이 과정에서도 top-down 형식의 탐색이 일어나며, 
Measure 단계에서 측정된 크기를 이용하여 ViewGroup 안에있는 View들의 위치를 결정합니다.

### Draw
GPU에게 명령을 내려주는 단계입니다. View Tree에서 각각의 View에 대한 Canvas 객체가 생성되는데, 이전 Measure,
Layout 단계를 거쳐 결정된 View들의 크기와 위치에 대한 정보가 들어가게 됩니다. 그 후 GPU 내부에서 처리를 거쳐 유저에게 UI가 보여지게 됩니다. 

## **성능 최적화**
GPU에서 그래픽을 처리하는 과정은 비용이 큰 작업에 속함으로 성능을 최적화하기 위해서는 불필요한 Draw 과정을 생략하는 것이 핵심입니다. 

### 유저에게 보여지지 않는 불필요한 Background Color 제거
유저에게 보여지지 않더라도 background color 지정 시 onDraw 단계를 거치게 됩니다. 
이는 중복 draw에 해당함으로 보여지지 않는 background color를 제거하면 성능 향상에 도움이 됩니다.

### ConstraintLayout을 사용하여 ViewGroup의 depth 줄이기
xml View System에서 자식뷰가 새로 그려지게 되면 그 위에있는 ViewGroup 안에 있는 뷰도 같이 새로 그려지게 됩니다. 
depth를 최소화하면 View를 새로 고침할 때 불필요한 draw를 방지를 할 수 있습니다. 
구글에서는 이를 위해 기존 LinearLayout, RelativeLayout을 사용하기보다 ConstraintLayout을 사용하는 것을 권장하고 있습니다.

**참고**<br>
[Android Developer](https://developer.android.com/guide/topics/ui/how-android-draws)
