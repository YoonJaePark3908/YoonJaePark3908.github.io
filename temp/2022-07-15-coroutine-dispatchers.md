---
title: Kotlin Coroutine dispatchers 정리
author: jaepark
date: 2023-12-01 13:13:00 +0900
categories: [Kotlin]
tags: [coroutine]
pin: false
img_path: '/assets/img'
---
## **Dispacher란?**
코루틴이 어느 스레드 풀에서 작동할지 정해주는 역할을 합니다. Dispatcher의 종류는 Main, IO, Default, Unconfined 총 4가지가 있습니다.
> 스레드 풀 : 스레드를 미리 생성하고, 작업 요청이 발생할 때 마다 미리 생성된 스레드로 해당 작업을 처리하는 방식
{: .prompt-info }

## **Main**
Android 메인 스레드에서 코루틴을 실행하는 디스패처입니다. UI와 상호 작용하는 작업을 위해서 사용합니다.

## **IO**
디스크 또는 네트워크 I/O 작업을 실행하는데 최적화 되어있는 디스패처입니다. Dispachers.IO를 설정해주면 백그라운드 스레드에서 실행이 됩니다. 
기본으로 설정된 스레드 풀의 스레드의 개수는 64개입니다.

## **Default**
Default 디스패처는 메인 스레드 외부에서 CPU를 많이 사용하는 일을 수행하는 데에 최적화되어있습니다. 
주로 직렬화, 역직렬화하는 json 파싱 작업, list sort 작업이 대표적입니다. 스레드 풀의 스레드 개수는 CPU 개수만큼 설정되어 있습니다.

## **Unconfined**
Unconfined 디스패처는 특정 스레드를 한정하지 않습니다.


**참고**<br>
[Coroutine의 Dispatcher란 무엇인가?](https://kotlinworld.com/141)<br>
[Kotlin 공식 문서](https://kotlinlang.org/docs/coroutine-context-and-dispatchers.html)
