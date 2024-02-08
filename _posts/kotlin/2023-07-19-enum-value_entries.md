---
title: Kotlin Enum에서 value() 함수 대신에 entries를 사용하면 좋은점
author: jaepark
date: 2023-07-19 13:13:00 +0900
categories: [Kotlin]
tags: [paging3]
pin: false
img_path: '/assets/img'
---
## **values() 함수의 문제점**

기본적으로 values()는 변경 가능한(mutable) Array<E> type으로 반환하는데, 이는 매번 호출할 때 마다 Array를 할당하고 복제해야 합니다. 
이는 Java와 Kotlin에서 성능 문제의 원인이 됩니다. 자세한 성능 이슈의 예는 아래 링크 참조하시면 됩니다.
- [HttpStatus.resolve allocates HttpStatus.values() once per invocation](https://github.com/spring-projects/spring-framework/issues/26842)
- [Kotlin standard library](https://github.com/JetBrains/kotlin/blob/92d200e093c693b3c06e53a39e0b0973b84c7ec5/libraries/stdlib/jvm/src/kotlin/text/CharCategoryJVM.kt#L170)
- [kotlinx.serialization Enum deserializer](https://github.com/Kotlin/kotlinx.serialization/issues/1372)
- [MySQL JDBC Remove Enum.values() calls to avoid unnecessary array](https://github.com/Microsoft/mssql-jdbc/pull/1065)

## **해결방법**
kotiln 1.8.20 버전에서 beta로 제공하며 1.9.0에서 정식 릴리즈 된 'entries'를 사용하면 됩니다. entries가 values() 보다 더 나은 이점은 다음과 같습니다.

- 미리 할당된 리스트를 반환 합니다. (이는 항상 같은 리스트를 반환합니다.)
- 변경이 불가능합니다. (immutable)
- entries는 EnumEntries<E> type을 반환 하는데 이는 List<E>를 상속함으로 List의 기존 extention 함수를 사용할 수 있음과 동시에 custom을 할 수 있습니다.

**참고**  
[Kotlin Git 공식 문서](https://github.com/Kotlin/KEEP/blob/master/proposals/enum-entries.md#examples-of-performance-issues)  
[Medium Blog](https://engineering.teknasyon.com/kotlin-enums-replace-values-with-entries-bbc91caffb2a)
