---
layout: post
title: 시작하기 - 1. BasicSyntax(기본 문법)
categories: dev
tags: kotlin
---

Kotlin를 배우고자 마음먹고 Reference를 공부하면서 온라인 스터디 공유를 위해 정리를 시작했는데, 이걸 그냥 공부 입증에만 쓰기에는 아깝다는 생각이 들어 정리하여 블로그로 올리기로 마음먹었다. 다만 FM으로 번역하기보다 내 이해를 돕기 위해 하는 것이니만큼 약간의 의역 + 주관적인 내용이 조금씩 포함될 수 있다.
앞으로 조금씩 번역, 정리해서 올릴 예정이다.

---

[원문: Getting Started - Basic Syntax](htps://kotlinlang.org/docs/reference/basic-syntax.html)

{% include_relative kotlin-reference-index.md %}

## 목차
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## 패키지 정의 및 import

패키지 명세는 소스 파일의 가장 위에 와야 한다.
```kotlin
package my.demo
import kotlin.text.*

// ...
```
폴더 구조와 패키지가 동일할 필요는 없다. 소스 파일은 파일 시스템 내에서는 임의적으로 배치될 수 있다.

## 프로그램 시작점
코틀린 앱의 시작점은 main 함수다.
```kotlin
fun main() {
    println("Hello world!")
}
```
## 함수
파라미터 타입과 리턴 타입은 다음과 같이 표시한다.
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```
표현식(expression)으로 나타낼 수도 있다.
```kotlin
fun sum(a: Int, b: Int) = a + b
```
## 변수
읽기 전용(자바에서 final) 변수는 val 키워드로 선언한다.
```kotlin
val a: Int = 1
val b = 2
```
한번 값을 할당하면 변경할 수 없다.

재할당 가능한 변수는 var 키워드를 사용한다.
```kotlin
var x = 5
x += 1
```

## 문자열 템플릿 사용하기
```kotlin
var a = 1
val s1 = "a is $a"
a = 2
val s2 = "${s1.replace("is", "was")}, but now is $a"
```
## 조건문
```kotlin
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
```
표현식으로도 사용할 수 있다.
```kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```
## null이 될 수 있는 값과 null 체크
참조가 null이 될 수 있을 경우 명시적으로 표시해 주어야 한다.
```kotlin
fun parseInt(str: String): Int? {
    // ...
}
```
null이 될 수 있는 값을 사용하는 함수
```kotlin
fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // x, y가 null값일 수 있기 때문에 x * y는 에러를 발생시킨다.
    if (x != null && y!= null) {
        // null 체크로 인해 null이 될 수 없도록 캐스팅된다.
        println(x * y)
    }
    else {
        println("'$arg1' or '$arg2' is not a number")
    }
}
```

## 타입 검사와 자동 형변환
'is' 연산자를 사용해서 표현식이 특정 타입의 객체인지 확인할 수 있다. 변경할 수 없는 지역변수나 속성을 확인 시 해당 타입으로 자동 형변환된다.
```kotlin
fun getStringLength(obj: Any): Int? {
    // && 앞의 연산에서 String으로 형변환됨
    if (obj is String && obj.length > 0) {
        return obj.length
    }
    return null
}
```
## for
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
for (item in items) {
    println(item)
}
```
혹은
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
for (index in items.indices) {
    println("item at $index is ${items[index]})
}
```
## while
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
var index = 0
while (index < items.size) {
    println("item at $index is ${item[index]})
}
```
## when
```kotlin
fun describe(obj: Any): String = 
    when (obj) {
        1           -> "One"
        "Hello"     -> "Greeting"
        is Long     -> "Long"
        !is String  -> "Not a string"
        else        -> "Unknown"
    }
```
## Ranges
'in'을 사용하여 숫자가 범위 내에 있는지 확인 가능하다.
```kotlin
val x = 10
val y = 9
if (x in 1..y+1) {
    println("fits in range")
}
```
범위를 벗어났는지 확인하기
```kotlin
val list = listOf("a", "b", "c")

if (-1 !in 0..list.lastIndex) {
    println("-1 is out of range)
}
if (list.size !in list.indices) {
    println("list size is out of valid list indices range")
}
```
범위 순회하기
```kotlin
for (x in 1..10 stp 2) {
    print(x)
}
println()
for (x in 9 downTo 0 step 3) {
    print(x)
}
```
## Collections
컬렉션 순회
```kotlin
for (item in items) {
    println(item)
}
```
컬렉션 원소 확인
```kotlin
when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}
```
람다 표현식으로 filter, map 사용하기
```kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
    .filter { it.startsWith("a")}
    .sortedBy { it }
    .map { it.toUpperCase() }
    .forEach { println(it) }
```
## 클래스와 인스턴스 만들기
```kotlin
val rectangle = Rectangle(5.0, 2.0) // new 키워드 사용하지 않음
val triangle = Triangle(3.0, 4.0, 5.0)
```