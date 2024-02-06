---
title: Kotlin ==, === 연산에 대해 정리
author: jaepark
date: 2023-11-14 22:15:00 +0900
categories: [Kotlin]
tags: [kotlin, equals]
pin: false
img_path: '/assets/img'
---
## **[공식 문서](https://kotlinlang.org/docs/equality.html)에 나온 Equality 내용**
- 코틀린에서 == 연산은 데이터 값이 같은지 판별하며 equlse()를 호출 합니다.
- 코틀린에서 === 연산은 주소 값이 같은지 판별합니다.
- 만약, a == null 연산을 했다면 자동으로 a === null 변환이 됩니다.
- 컴파일시에 원시형으로 변환 되는 타입(Int, Double...)에서 == 연산과 === 연산의 동작은 같습니다.
- Array 객체의 원소를 비교하려면 contentEquals()를 사용합니다.

공식문서에 나온 개념을 바탕으로 실험을 했습니다.
## **실험 1. data class 객체의 ==와 === 비교**
```kotlin
data class People(
    val name: String,
    val age: Int
)

fun main() {
    val a = People(name = "a", age = 51)
    val b = People(name = "a", age = 51)

    println("== 실험 : ${a == b}")
    println("=== 실험 : ${a === b}")
}

/**출력
== 실험 : true
=== 실험 : false
**/
```

a, b의 data class 객체의 데이터는 같기 때문에 == 연산을 하면 true를 반환하지만, 할당된 주소는 다르기 때문에 === 연산은 false를 반환하는 것을 확인할 수 있었습니다.

## **실험 2. 컴파일 시 원시 타입으로 변환 되는 Int의 ==와 ===비교**
```kotlin
fun main() {
  val a = 1
  val b = 1

  println("== 실험 : ${a == b}")
  println("=== 실험 : ${a === b}")
}

/**출력
== 실험 : true
=== 실험 : true
**/
```

공식 문서에 나온대로 원시 타입으로 변환 되는 Int 같은 경우는 ==, === 연산이 같은 개념으로 동작한다는 것을 알 수 있었습니다. 

## **실험3. Array 원소 비교**
```kotlin
fun main() {
  val a = arrayOf(1,2,3,4,5)
  val b = arrayOf(1,2,3,4,5)

  println("== 실험 : ${a == b}")
  println("=== 실험 : ${a === b}")
  println("contentEquals 실험 : ${a.contentEquals(b)}")
}

/**출력
== 실험 : false
=== 실험 : false
contentEquals 실험 : true
**/
```
Array의 데이터를 비교할 때는 contentEqulse를 사용하라고 명시 돼있는데 이 확장함수 내부를 보면 java에서 Arrays에 정의 된 equals를 불러와서 확인하는것을 알 수 있습니다. 
```kotlin
@SinceKotlin("1.4")
@JvmName("contentEqualsNullable")
@kotlin.internal.InlineOnly
public actual inline infix fun <T> Array<out T>?.contentEquals(other: Array<out T>?): Boolean {
    return java.util.Arrays.equals(this, other)
}
```
