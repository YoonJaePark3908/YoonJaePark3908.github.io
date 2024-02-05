---
title: Java에서 Kotlin으로 넘어갔을 때 느낀점
author: jaepark
date: 2021-06-22 17:15:00 +0900
categories: [Kotlin]
tags: [kotlin]
pin: true
img_path: '/assets/img'
---

## **편리하고 간결한 코드**

Java에서 Kotlin으로 넘어가면서 가장 처음 느낀점은 편리하고 간결하다입니다. 왜 그렇게 느꼈는지 몇가지 이유를 적어봤습니다.

### **Null Safety Check**

제가 Android 개발에서 Java를 사용했을 때 버전 8기준으로 Null Safety하게 다루는 방법은 if문으로 null을 체크하는 방식이였습니다.

``` java
if (instance != null) {
    instance.runMethod()
}
```

하지만 코틀린에서는 이런 if문을 생략하고도 null safety하게 코드를 작성할 수 있었습니다.

``` kotlin
instance?.runMethod()
```

이렇게 코드를 작성하면 instance가 null일경우 NullPointException을 반환하지 않고 아무 동작없이 넘어가는것을 확인할 수 있습니다.

### **Data class**

Java에서는 데이터 모델 Class를 만들 때 하나하나 모든것을 정의를 해줘야 했습니다. 특히 get과 set함수,
toString 등등 정의 해줘야할것이 여러개였는데 이를 한번에 해결해주는 data class가 있었습니다.
data class는 캡슐화로 인한 get과 set을 따로 정의할 필요가 없으며 toString, hashCode, copy, componantN, equls()를 자동으로 생성해줍니다.
다음은 data class를 만들 때 Java와 Kotlin의 차이점입니다.

- Java

``` java
class Model {
   private String name;
   private String age;
   
   public void setName(String name) {
      this.name = name;
   }
   
   public String getName() {
      retrun this.name;
   }
   
   public void setAge(String age) {
      this.age = age;
   }
   
   public String getAge() {
      return this.age
   }
   
   @NonNull
   @Override
   public String toString() {
      return "Model(name = " + name + ", age = " + age + ")";
   }
   
   ....
}
```

- Kotlin

```kotlin
data class Model(
  val name: String = "",
  val age: String = ""
)
```

### **타입 추론**

Kotlin은 타입 추론이 되는 언어입니다. 컴파일러가 알아서 타입을 추론해주기 때문에 변수를 선언할 때 타입을 따로 지정해주지 않는 간편함이 있습니다.

- Java

``` java
final String name = "";
int age = 0;
```

- Kotlin

``` kotlin
val name = ""
var age = 0
```

### **간단한 Singleton 생성**
Kotlin에서는 object를 통해 간단하게 SingleTon의 Class를 생성할 수 있으며 Kotlin의 object는 thread safety하게 
설계 되어있어 따로 코드를 작성하지 않아도 됩니다.
- Java

``` java
public class Singleton {
	private static Singleton instance;
	
	private Singleton(){}
	
	public static synchronized Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

- kotlin

``` kotlin
object Singleton {
  //TODO write code	
}
```

## **낮은 러닝 커브**

kotlin은 JVM 기반 언어이기 때문에 java와 호환성이 좋은 장점이 있습니다. 기존에 java언어를 해온 사람의 입장에서는 러닝커브가 낮은 편이며 
배우는 재미가 있었습니다. Java를 사용하면 비동기 처리를 위해 러닝커브가 높은 편인 RxJava를 사용하는것에 비해 Kotlin을 사용하면 비동기 처리는 
대부분 Coroutine을 사용합니다. Coroutine은 러닝커브가 다른 비동기 처리에 비해 러닝커브가 낮은 편에 속합니다. 
하지만 대부분의 공부가 그렇듯이 더 깊게 공부하면 러닝 커브는 높아지고 알아야 할것은 많아질 것입니다. 
이제는 Android의 공식 언어가 된 Kotlin을 할때라고 생각하며 포스팅을 마치겠습니다.  
감사합니다!
