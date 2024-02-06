---
title: Kotlin object와 companion object에 대해 공부
author: jaepark
date: 2022-07-21 10:15:00 +0900
categories: [Etc]
tags: [kotlin object]
pin: false
img_path: '/assets/img'
---
## **Object**
kotlin에서 객체를 싱글톤으로 동작하고 싶으면 간단하게 object 키워드를 사용하여 구현할 수 있습니다. 
- 싱글톤 패턴 : 메모리상에 단 하나만의 객체를 생성하기 위해 만드는 패턴

```kotlin
object MyObject {
  fun printHello() {
    println("Hello World!")
  }
}

fun main() {
  MyObject.printHello()
}

/**실행 결과
Hello World!
**/
```
object 키워드는 thread-safe 하며 늦은 초기화를 합니다. 즉, 여러 쓰레드가 동시에 접근해도 안전하게 처리하며, object가 사용될 때까지 초기화를 늦추는 특성이 있습니다.

## **companion object**

class 내부에서 객체를 생성하지 않고 호출 가능하도록 만드는데 사용합니다. companion object는 클래스 내부에서만 생성이 가능하며, 
companion object가 선언된 class가 초기화 되는 시점에서 같이 초기화가 진행됩니다.

```kotlin
class MyClass {
    companion object {
        fun printHello() {
            println("Hello World!")
        }
    }
}

fun main() {
  MyClass.printHello()
}

/**실행 결과
Hello World!
**/
```

## **차이점**
둘의 명확한 차이점은 초기화 시점에 있습니다. object는 object를 불러올 때까지 초기화를 늦추는 반면, companion object는 선언된 class가 초기화되는 시점에 같이 초기화가 됩니다.

