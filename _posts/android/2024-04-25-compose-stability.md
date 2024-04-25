---
title: Android Compose의 Stability
author: jaepark
date: 2024-04-25 11:04:00 +0900
categories: [Android]
tags: [compose]
pin: false
img_path: '/assets/img'
---
## Compose의 Stability
**Compose는 Recompose 시 불안정한 타입의 값을 가진 Componant는 Recompose가 되고, 안정한 타입을 가진 Componant는 Recompose가 스킵이 됩니다.**<br> 
Android 앱의 성능을 향상 시키기 위해서는 불필요한 UI 렌더링을 피하는것이 기본임으로 Compose를 사용하여 더 좋은 앱을 만들기 위해서는 불필요한 
Recompose를 피해야 합니다. 공식 문서에서는 Data class를 예시로 Stable, Unstable을 설명 하고 있습니다.

### 불변의 객체
data class를 다음과 같이 primary constructor를 val 키워드로 정의하면 안정한 타입으로 간주하여 Recompose 대상에서 제외가 됩니다.<br>
```kotlin
data class Contact(val name: String, val number: String)
```
정말 그런지 다음과 같이 코드를 작성하여 실험을 통해 알아 봤습니다. 
```kotlin
Column(modifier = Modifier.padding(25.dp)) {
    var selected by remember { mutableStateOf(false) }
    ContactDetails(Contact(name = "윤재박", number = "01012345678"))
    Switch(selected, onCheckedChange = { selected = !selected })
}
```

![stable_example.gif](/android/compose/stability/stable_example.gif){: width="300"}
![skip_recompose.png](/android/compose/stability/skip_recompose.png){: width="700"}

위와 같이 ContactDetails의 Component는 Recompose가 Skip 되는것을 확인 할 수 있었습니다.<br>
### 변경 가능한 객체
반대로, data class를 다음과 같이 primary constructor를 var 키워드로 정의하면 불안정한 타입으로 간주하여 Recompose 대상이 됩니다.<br>
```kotlin
data class Contact(var name: String, var number: String)
```
위에 나온 예시대로 var 키워드를 정의하여 직접 Recompose의 여부를 확인해봤습니다.<br>
![image_2.png](/android/compose/stability/image_2.png){: width="800"}
공식 문서에 나온대로 var 키워드를 선언했을 때 ContactDetails Componant는 Recompose가 되는것을 확인할 수 있었습니다.<br>
만약 이러한 변경 가능한 객체를 안전한 타입으로 설정하고 싶을때 **@Stable** 어노테이션을 사용하면 var 키워드를 주더라도 Recompose 대상에서 제외가 가능합니다.

## 참고
[https://developer.android.com/develop/ui/compose/performance/stability](https://developer.android.com/develop/ui/compose/performance/stability)
