---
title: Android Compose에서 Image/Icon Recompose 줄이기
author: jaepark
date: 2023-10-11 15:24:00 +0900
categories: [Android]
tags: [compose]
pin: false
img_path: '/assets/img'
---
> [Medium 글](https://engineering.teknasyon.com/reduce-recomposition-for-images-icons-in-jetpack-compose-8d2dd3bfa933)을 읽다 성능 관련 꿀팁이라 공유합니다.
>

## **요약**
Compose의 Image / Icon에서 이미지 리소스를 지정할 때 painter 대신에 imageVector를 쓰면 불필요한 recompose를 줄일 수 있습니다.
```kotlin
//Before
Image(
   painter = painterResource(id = R.drawable.ic_launcher_background),
   contentDescription = null
)

//After
Image(
   imageVector = ImageVector.vectorResource(id = R.drawable.ic_launcher_background),
   contentDescription = null
)
```

한 가지 주의할 점은 ImageVector는 @Immutable 어노테이션이 선언 됐기 때문에 stable한 type으로 간주하는 class입니다. 
이 때문에 recompose 트리거가 오더라도 ImageVector로 선언된 Image/Icon은 recompose에서 제외됩니다. 
하지만, 앱 시나리오 중에 image 리소스가 바뀔 수 있는 부분이 있다면 painter 인자를 선언 해주시면 됩니다.
