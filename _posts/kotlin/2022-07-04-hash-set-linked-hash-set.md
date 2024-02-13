---
title: Kotlin HashSet과 LinkedHashSet의 차이
author: jaepark
date: 2022-07-04 22:31:00 +0900
categories: [Kotlin]
tags: [set]
pin: false
img_path: '/assets/img'
---
코틀린에서 HashSet을 생성하기 위해 hashSetOf를 사용하며, LinkedHashSet을 생성하기 위해 setOf를 사용합니다. 
setOf를 자세히 들여다보면 LinkedHashSet으로 만드는 것을 볼 수 있습니다.

```kotlin
public fun <T> setOf(vararg elements: T): Set<T> = if (elements.size > 0) elements.toSet() else emptySet()

public fun <T> Array<out T>.toSet(): Set<T> {
    return when (size) {
        0 -> emptySet()
        1 -> setOf(this[0])
        else -> toCollection(LinkedHashSet<T>(mapCapacity(size)))
    }
}
```
HashSet은 요소를 추가한 순서를 저장하지 않고 LinkedHashSet은 요소를 추가한 순서를 저장합니다.
코드 결과로 보면 다음과 같습니다.
```kotlin
val hashSet = hashSetOf("apple","banana","grape")
hashSet.add("orange")
hashSet.add("melon")

val linkedHashSet = mutableSetOf("apple","banana","grape")
linkedHashSet.add("orange")
linkedHashSet.add("melon")

println("hashSet = $hashSet")
println("linkedHashSet = $linkedHashSet")
    
//출력
hashSet = [banana, orange, apple, grape, melon]
linkedHashSet = [apple, banana, grape, orange, melon]
```
이렇게 되면 LinkedHasySet은 요소의 순서를 저장하기 때문에 index를 통해서 접근할 수 있을것 같지만, 
Set자체는 index로 객체를 관리하지 않기 때문에 index를 통한 접근이 불가능합니다.
