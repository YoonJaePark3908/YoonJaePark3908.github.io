---
title: Kotlin Data Class 공식 문서 살펴보기
author: jaepark
date: 2023-06-14 23:41:00 +0900
categories: [Kotlin]
tags: [data class]
pin: false
img_path: '/assets/img'
---
## **Data classes**
data class는 데이터를 담기위한 목적으로 만들어진 클래스이며, 데이터 클래스를 다음과 같이 정의하면 컴파일러는 다음과 같은 함수를 자동으로 생성합니다.
```kotlin
data class User(val name: String = "John", val age: Int = 42)
```
- .equals() / .hashCode()
- .toString() : data class toString()은 다음과 같이 출력 됩니다. "User(name=John, age=42)"
- .componentN() 함수 : data class에서 선언한 순서대로 정의되는 함수입니다. 예를들어 componet1()은  name의 값을 return 합니다.
- .copy() : data class의 속성 값을 복사합니다. 이미 정의 돼 있는 일부 데이터를 변경할 때 주로 사용합니다.

이러한 자동으로 generated 되는 코드의 일관성과 의미있는 동작을 보장하기 위해서 다음과 같은 제약사항이 있습니다.

- data class의 primary constructor는 최소 한개의 파라미터를 가져야 합니다.
- 모든 primary constructor의 파라미터는 val 또는 var 키워드로 선언돼야 합니다.
- data class는 abstract, open, sealed, inner를 선언할 수 없습니다.

> primary constructor에 대해 쓴 좋은 글을 보시려면 [여기](https://readystory.tistory.com/124)로 들어가서 읽으시면 됩니다.
> {: .prompt-tip }

## **실험**
### data class 내부에 선언한 변수도 자동으로 함수를 만들어줄까?
결론은 아닙니다.

```kotlin
data class User(
    val name: String = "John",
    val age: Int = 42
) {
    val isNameJohn = name == "John"
}

fun test() { 
  val user = User().copy(name = "Jaepark")
  println(user.toString())
  println(user.isNameJohn)
}
```

```console
User(name=Jaepark, age=42)
false
```

user data class를 자동으로 생성된 toString을 출력 해본결과 data class 내부에 정의된 isNameJohn은 출력되지 않았습니다.

#### 프로퍼티 : 클래스 안에서 정의된 변수
참고  
[Kotlin 공식 문서 - data classes](https://kotlinlang.org/docs/data-classes.html)  
[Kotlin 공식 문서 - collections copy](https://kotlinlang.org/docs/constructing-collections.html#copy)  
[코틀린 생성자(Kotlin Constructor) 제대로 이해하기](https://readystory.tistory.com/124)
