---
layout: post
title: 기본 - 2. 제어 흐름
categories: dev
tags: kotlin
---


[원문: Basics - Control Flow](https://kotlinlang.org/docs/reference/control-flow.html)

{% include_relative kotlin-reference-index.md %}

## 목차
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## if 표현식

코틀린에서 `if`는 값을 반환할 수 있는 '표현식'이다. 따라서 `if`가 동일한 역할을 수행할 수 있으므로 삼항 연산자(조건 ? true : false)는 존재하지 않는다.

```kotlin
// 기존의 사용법
var max = a
if (a < b) max = b

// else와 함께
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}

// 표현식으로
val max = if (a > b) a else b
```

`if`의 분기는 블록이 될 수 있으며 마지막 표현식이 블록의 값이 된다.
```kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```

`if`를 문장보다는 표현식으로 사용한다면(값을 반환한다던지 변수에 할당한다던지), 표현식은 else 분기를 가져야 한다.

[`if`의 문법](https://kotlinlang.org/docs/reference/grammar.html#ifExpression)을 보라.

## when 표현식

`when`은 C 계열 언어의 switch 연산자를 대신한다. 가장 간단한 형태는 다음과 같다.
```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("y == 2")
    else -> {   // 블록을 주목하라
        print("x는 1도 아니고 2도 아니다")
    }
}
```

`when`은 분기 조건을 만족할 때까지 인자와 모든 분기를 순차적으로 비교한다. `when`은 표현식으로도 문장으로도 사용할 수 있다. 표현식으로 사용될 경우 조건을 만족하는 분기의 값이 전체 표현식의 값이 된다. 문장으로 사용될 경우에는 각 분기의 값은 무시된다.(`if`처럼 각 분기는 블록이 될 수 있고 블록의 마지막 표현식이 값이 된다.)

`else` 분기는 다른 어떤 분기도 조건을 만족하지 못할 경우 계산된다. `when`이 표현식으로 사용될 경우, 컴파일러가 분기 조건이 모든 가능한 경우를 다룬다고 증명하지 않는 한(예. enum 클래스의 엔트리, sealed 클래스의 서브타입) else 분기는 의무사항이다.

많은 경우가 같은 방식으로 처리될 경우, 분기 조건은 콤마(,)로 연결하여 작성할 수 있다.
```kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

상수뿐만 아니라 임의의 표현식을 분기 조건으로 사용할 수 있다.
```kotlin
when (x) {
    parseInt(s) -> print("s는 x를 암호화한다")
    else -> print("s는 x를 암호화하지 않는다.)
}
```

`in`이나 `!in`을 사용해서 값이 [*범위](https://kotlinlang.org/docs/reference/ranges.html) 또는 컬렉션 안에 있는지 검사할 수 있다.
```kotlin
when (x) {
    in 1..10 -> print("x가 범위 안에 있음")
    in validNumbers -> print("x는 유효함")
    !in 10..20 -> print("x가 범위 밖에 있음")
    else -> print("해당 없음")
}
```
또한 `is`나 `!is`로 값이 특정 타입인지 검사할 수 있다. [*스마트 캐스트](https://kotlinlang.org/docs/reference/typecasts.html#smart-casts) 로 인해 추가적인 검사 없이 타입의 메서드와 프로퍼티에 접근할 수 있게 된다.
```kotlin
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```

또한 `when`은 `if-else if` 연쇄를 대신하여 사용될 수 있다. 인자가 전해지지 않는다면 분기 조건은 단순한 참 거짓 표현식이 되며 분기는 조건이 참일 때 실행된다.
```kotlin
when {
    x.isOdd() -> print("x는 홀수")
    x.isEven() -> print("x는 짝수")
    else -> print("x는 별종")
}
```

코틀린 1.3부터 다음 구문을 사용해 `when`의 제목 부분에서 연산을 변수에 캡쳐할 수 있다.  
```kotlin
fun Request.getBody() =
    when (val response = executeRequest()) {
        is Success = response.body
        is HttpError -> throw HttpException(response.status)
    }
```

`when`의 제목 부분에 선언된 변수는 사용이 `when` 몸체 부분으로 제한된다.

[`when` 문법](https://kotlinlang.org/docs/reference/grammar.html#whenExpression)을 보라.


## for 반복문

`for` 반복문은 반복자(iterator)를 제공하는 모든 것을 순회할 수 있다. 이는 C#과 같은 언어의 `foreach`문과 같은 역할을 한다. 구문은 다음과 같다:
```kotlin
for (item in collection) print(item)
```

몸체는 블록이 될 수 있다.
```kotlin
for (item: Int in ints) {
    // ...
}
```

직전에 언급했듯이 `for`는 반복자를 제공하는 모든 것을 순회할 수 있다. 예를 들면,
- `iterator()`를 멤버 함수 또는 확장 함수로 가지면서, 리턴 타입이
    - `next()`를 멤버 함수 또는 확장 함수를 가지며,
    - `Boolean`을 리턴하는 `hasNext()`를 멤버 함수 또는 확장 함수를 가지는 경우

이 세 가지 함수 모드 `operator`로 표시되어야 한다.

숫자의 범위를 순회하기 위해서는 [범위 표현식](https://kotlinlang.org/docs/reference/ranges.html)을 사용하라:
```kotlin
for (i in 1..3) {
    println(i)
}
for (i in 6 downTo 0 step 2) {
    println(i)
}
```

범위나 배열을 순회하는 `for` 반복문은 인덱스를 기반으로 하는 반복문으로 컴파일되며 반복자 객체를 생성하지 않는다.

인덱스를 통해 배열이나 리스트를 순회하고 싶을 때는 아래와 같은 방법으로 가능하다:
```kotlin
for (i in array.indices) {
    println(array[i])
}

대안으로 `withIndex` 라이브러리 함수를 사용할 수 있다.
```kotlin
for ((index, value) in array.withIndex()) {
    println("$index의 요소는 $value이다.")
}

[`for` 문법](https://kotlinlang.org/docs/reference/grammar.html#forStatement)을 확인하라.


## while 반복문

## 반복문에서의 break와 continue