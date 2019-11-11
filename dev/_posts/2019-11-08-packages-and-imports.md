---
layout: post
title: 기본 - 2. 패키지와 임포트
categories: dev
tags: kotlin
---


[원문: Basics - Packages and Imports](https://kotlinlang.org/docs/reference/packages.html)

{% include_relative kotlin-reference-index.md %}

## 목차
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## 패키지

소스 파일은 패키지 선언으로 시작한다.
```kotlin
package org.example

fun printMessage() { /*...*/ }
class Message { /*...*/ }

// ...
```

소스 파일의 모든 내용(클래스나 함수)은 패키지 선언에 포함된다. 따라서 위 예제에서 `printMessage()`의 전체 이름은 `org.example.printMessage`이며, `Message`의 전체 이름은 `org.example.Message`가 된다.

패키지가 명시되지 않은 파일의 내용은 이름이 없는 기본 패키지(default package) 소속이 된다.

### 기본 임포트

몇 가지 패키지는 모든 코틀린 파일에 기본으로 임포트된다.

- kotlin.*
- kotlin.annotation.*
- kotlin.collections.*
- kotlin.comparisons.* (1.1버전부터)
- kotlin.io.*
- kotlin.ranges.*
- kotlin.sequences.*
- kotlin.text.*

추가적인 패키지들은 대상 플랫폼의 종류에 따라 임포트된다.

- JVM:
    - java.lang.*
    - kotlin.jvm.*
- JS:
    - kotlin.js.*

### 임포트

기본 임포트와 별개로, 각각의 파일은 고유의 임포트 지시문을 포함할 수 있다. 임포트 구문은 [*문법](https://kotlinlang.org/docs/reference/grammar.html)에 설명되어 있다.

하나의 이름만을 임포트하거나,
`import org.example.Message //  패키지까지 적지 않고 Message 사용 가능`
범위 내(패키지, 클래스, 객체 등)의 모든 내용을 임포트할 수도 있다.
`import org.example.*   //'org.example'의 모든 항목 사용 가능`

이름이 동일한 요소가 있다면 `as` 키워드로 파일 내에서 별명을 지어줄 수 있다.
```kotlin
import org.example.Message // Message에 사용 가능
import org.text.Message as testMessage // testMessage로 'org.text.Message' 사용 가능
```

`import` 키워드로 클래스뿐만 아니라 다른 선언들 역시 불러올 수 있다.

- 최상위 함수와 프로퍼티
- [*객체 선언](https://kotlinlang.org/docs/reference/object-declarations.html#object-declarations)에 선언된 함수와 프로퍼티
- [열거형(enum) 상수](https://kotlinlang.org/docs/reference/enum-classes.html)

### 최상위 선언의 접근성

최상위 선언이 private일 경우, 선언되어 있는 파일에 대해 private이다([접근 제한자](https://kotlinlang.org/docs/reference/visibility-modifiers.html) 참고)