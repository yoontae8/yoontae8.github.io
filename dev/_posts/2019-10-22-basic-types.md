---
layout: post
title: 기본 - 1. 기본 자료형
categories: dev
tags: kotlin
---


[원문: Basics - Basic Types](https://kotlinlang.org/docs/reference/basic-types.html)

{% include_relative kotlin-reference-index.md %}

## 목차
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

코틀린에서는 어떤 변수에서든 멤버 함수와 프로퍼티를 호출할 수 있으므로 모든 것이 객체라고 말할 수 있다. 일부 자료형은 내부적으로 특별하게 표현되지만 - 예를 들어, 숫자와 문자, 불리언 값은 런타임에서 기본형으로 표현된다 - 사용자에게는 보통 클래스처럼 보인다. 이 절에서는 코틀린에서 사용하는 기본 자료형, 즉 숫자(number), 문자(character), 불리언(boolean), 배열(arrays), 그리고 문자열에 대해 설명한다.

## 숫자

코틀린은 숫자를 표현하는 일련의 내장된 타입을 제공한다.

각기 다른 크기, 즉 다른 범위를 위한 네 가지 자료형으로 정수형 숫자를 나타낼 수 있다.

자료형 | 크기(비트) | 최소값 | 최대값
----- | ------ | ----- | -----
Byte | 8 | -128 | 127
Short | 16 | -32768 | 32767
Int | 32 | -2,147,483,648(-2<sup>31</sup>) | 2,147,483,647(2<sup>31<sup>-1)
Long | 64 | -9,223,372,036,854,775,808(-2<sup>63</sup>) | 9,223,372,036,854,775,807(2<sup>63</sup>-1)

`Int`의 최대값을 넘지 않는 정수값으로 초기화되는 모든 변수는 `Int`형으로 추론된다. 초기값이 이 값을 초과하게 되면 `Long` 형이 된다. `Long` 형임을 명시적으로 표시하고 싶다면 값에 접미사 L을 추가하면 된다.
```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```

부동소수형 숫자를 위해서 코틀린은 `Float`과 `Double`을 제공한다. [IEEE 754 standard](https://en.wikipedia.org/wiki/IEEE_754)에 따르면 부동소수형은 소수 자리에 따라, 즉 소수 자리수를 얼마나 저장할 수 있는지에 따라 구분된다. `Float`은 *IEEE 754 단정도* 를 반영하고 `Double`은 *배정도* 를 제공한다.

자료형 | 크기(비트) | 가수부 | 지수부 | 십진 자리수
----- | ------ | ----- | ----- | -----
Float | 32 | 24 | 8 | 6-7
Double | 64 | 53 | 11 | 15-16

분수로 초기화된 변수는 `Double` 형으로 추론된다. 값에 `Float` 형을 명시적으로 적용하기 위해서는 접미사 `f` 또는 `F`를 추가한다. 값의 자리수가 6-7자리를 초과한다면 반올림된다.

```kotlin
val pi = 3.14 // Double
val e = 2.7182818284 // Double
val eFloat = 2.7182818284F // Float, 값은 2.7182817이 된다.
```

다른 언어들과는 달리 코틀린에서는 암시적인 확대 변환이 이루어지지 않는다는 것을 유념하라. 예를 들어, `Double` 파라미터를 받는 함수는 `Float`도 `Int`도 다른 어떤 숫자값도 아닌 오직 `Double` 값으로만 호출할 수 있다.
```kotlin
fun main() {
    fun printDouble(d: Double) {
        print(d)
    }

    val i = 1
    val d = 1.1
    val f = 1.1f

    printDouble(d)
//  printDouble(i)  // Error: 타입 불일치
//  printDouble(f)  // Error: 타입 불일치
}
```
숫자값을 다른 형으로 변환하기 위해서 [명시적 변환](#명시적-변환)을 사용하라.

### 리터럴 상수

정수값을 위한 리터럴 상수의 종류는 다음과 같다.

- 십진수: `123`
  - Long 값은 대문자 `L`을 붙인다: `123L`
- 16진수: `0x0F`
- 2진수: `0b00001011`
> 참고: 8진수 리터럴은 지원되지 않는다.

코틀린은 부동소수의 관용적 표기법 또한 지원한다.

- 기본형인 Double: `123.5`, `123.5e10`
- Float은 `f` 혹은 `F`를 붙인다: 123.5f

### 숫자 리터럴에서 언더스코어의 사용(1.1부터)

언더스코어(`_`)를 사용해서 상수를 읽기 쉽게 표현할 수 있다.

```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

### 표현

자바 플랫폼에서, null이 될 수 있는 숫자 레퍼런스(예. `Int?`)를 필요로 하거나 제네릭이 관련되지 않는 이상 숫자는 물리적으로 JVM 기본자료형으로 저장된다. 후자의 경우 숫자는 박싱된다.

숫자를 박싱하게 되면 그 값의 동일성(identity)을 보장할 수 없게 됨을 유념하라.

```kotlin
val a: Int = 10000
println(a === a) // 'true' 출력
val boxedA: Int? = a
val anotherBoxedA: Int? = a
println(boxedA === anotherBoxedA) // !!!'false' 출력!!!
```

반면에, 동등성(equity)는 보장된다.
```kotlin
val a: Int = 10000
println(a == a) // 'true' 출력
val boxedA: Int? = a
val anotherBoxedA: Int? = a
println(boxedA === anotherBoxedA) // 'true' 출력
```

### 명시적 변환

표현 방식이 다르기 때문에, 작은 자료형은 큰 자료형의 하위형이 아니다. 만약 그랬다면 다음과 같은 부분에서 문제가 생겼을 것이다.
```kotlin
// 가상의 코드. 실제로 컴파일되지 않음
val a: Int? = 1 // 박싱된 Int (java.lang.Integer)
val b: Long? = a // 암시적 형변환된 박싱된 Long (java.lang.Long)
print(b == a) // Long의 equals()는 타입이 Long인지도 체크하기 때문에 "false"를 출력한다.
```

이와 같이 온갖 곧에서 동일성과 동등성이 훼손되는 문제가 발생했을 것이다.

결과적으로, 작은 자료형은 큰 자료형으로 암시적 형변환되지 않는다. 이것은 명시적 형변환 없이는 `Byte`형의 값을 `Int` 변수에 할당할 수 없음을 의미한다.
```kotlin
val b: Byte = 1 // 가능하다. 리터럴은 정적으로 검사된다.
val i: Int = b // 에러 발생.
```

숫자를 확장하기 위해 명시적 형변환을 사용할 수 있다.
```kotlin
val i: Int = b.toInt()  // 가능하다. 명시적으로 확장되었다.
print(i)
```

모든 숫자 타입은 다음과 같은 변환을 지원한다.

- toByte(): Byte
- toShort(): Short
- toInt(): Int
- toLong(): Long
- toFloat(): Float
- toDouble(): Double
- toChar(): Char

문맥에서 자료형을 유추할 수 있고, 또 산술 연산은 적합한 변환으로부터 오버로딩되기 때문에, 암시적 형변환을 지원하지 않는다는 사실은 인식하기 어렵다.
```kotlin
val l = 1L + 3 // Long + Int => Long
```

### 연산

코틀린은 숫자 계산을 위해서 적합한 클래스의 멤버로 선언된 일련의 기본 산술 연산을 제공한다(그러나 컴파일러가 호출에 대응하는 명령으로 최적화한다). [*연산 오버로딩](https://kotlinlang.org/docs/reference/operator-overloading.html)을 보라.

비트 연산을 위한 특별한 연산자는 없지만 중위 형식으로 사용하는 함수는 존재한다.
```kotlin
val x = (1 shl 2) and 0x000FF000
```

비트 연산의 목록은 다음과 같다(`Int`와 `Long` 한정 사용)

- `shl(bits)` - 부호를 고려한 왼쪽 시프트
- `shr(bits)` - 부호를 고려한 오른쪽 시프트
- `ushr(bits)` - 무부호 오른쪽 시프트
- `and(bits)` - **and** 비트 연산
- `or(bits)` - **or** 비트 연산
- `xor(bits)` - **xor** 비트 연산
- `inv()` - 반전(inversion) 비트 연산

### 부동소수 비교하기

이 절에서 다루는 부동소수에 대한 연산은 다음과 같다.

- 동등성 검사: `a == b` 와 `a != b`
- 비교 연산자: `a < b`, `a > b`, `a <= b`, `a >= b`
- 범위 초기화 및 범위 검사: `a..b`, `x in a..b`, `x !in a..b`

피연산자 `a`와 `b`가 정적으로 `Float` 혹은 `Double` 타입이거나 null이 될 수 있는 사본(타입 선언이나 추론 혹은 smart cast의 결과로)일 경우, 숫자에 대한 연산이나 범위는 부동소수에 대한 IEEE 754 표준을 따른다.

그러나 일반적인 사용에 대한 지원과 전체적인 정렬 위해서, 피연산자가 정적 부동소수 타입이 아닐 경우(예. `Any`, `Comparable<...>`, 타입 파라미터) `Float`과 `Double` 연산은 표준과는 다른 `equals`와 `compareTo` 구현을 사용하기 때문에 다음과 같은 특성을 지닌다.

- `NaN`은 자기 자신과 동일한 것으로 간주한다.
- `NaN`은 `POSITIVE_INFINITY`를 포함한 그 어떤 요소보다도 큰 것으로 간주한다.
- `-0.0`은 `0.0`보다 작은 것으로 간주한다.

## 문자

문자는 `Char` 타입으로 표현한다. 문자는 직접적으로 숫자로 취급될 수 없다.

```kotlin
fun check(c: char) {
    if (c == 1) {   // 에러: 호환되지 않는 타입
        // ...
    }
}
```

문자 리터럴은 작은따옴표로 표시한다: `'1'`. 특수문자들은 역슬래시로 표현할 수 있다. 다음과 같은 이스케이프 시퀀스(escape sequence)를 지원한다: `\t`, `\b`, `\n`, `\r`, `\'`, `\"`, `\\`, `\$`. 다른 어떤 문자를 인코딩하기 위해서는 유니코드 이스케이프 시퀀스를 사용하라: `'\uFF00'`.

명시적으로 문자를 `Int`형 숫자로 변환할 수 있다.
```kotlin
fun decimalDigitValue(c: Char): Int {
    if (c !in '0'..'9')
        throw IllegalArgumentException("Out of range")
    return c.toInt() - '0'.toInt() // 숫자로의 명시적 변환
}
```

숫자처럼, null이 될 수 있는 참조가 필요할 경우 문자는 박싱된다. 박싱 연산에서 동일성(identity)은 보존되지 않는다.

## 불리언

`Boolean` 타입은 불리언을 표현하며 두 가지 값을 지닌다: `true`, `false`

불리언은 null이 될 수 있는 참조가 필요한 경우 박싱된다.

불리언을 위한 내장된 연산은 다음과 같다.

- `||` - 지연 논리합
- `&&` - 지연 논리곱
- `!` - 부정

## 배열

코틀린에서 배열은 `Array` 클래스로 표현되며 연산자 오버로딩 관용법에 의해 `[]`로 변환되는 `get`과 `set` 함수, 그리고 `size` 프로퍼티와 다른 실용적인 멤버 함수를 제공한다.

```kotlin
class Array<T> private constructor() {
    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit

    operator fun iterator(): Iterator<T>
    // ...
}
```

배열을 생성하기 위해서는 라이브러리 함수인 `arrayOf()`에 아이템 값들을 넘겨 사용할 수 있다. 즉 `arrayOf(1, 2, 3)`은 배열 `[1, 2, 3]`을 생성한다. 이를 대신해서 라이브러리 함수 `arrayOfNulls()`는 주어진 크기의 null 요소로 채워진 배열을 생성한다.

다른 선택지는 배열 크기와 인덱스를 인자로 하여 각 배열 요소의 초기값을 반환하는 함수를 받는 `Array` 생성자를 이용하는 것이다:

```kotlin
// ["0", "1", "4", "9", "16"] 값의  Array<String>을 생성한다.
val asc = Array(5) { i -> (i * i).toString() }
asc.forEach { print(it) }
```

위에서 언급했듯이 `[]` 연산은 멤버 함수인 `get()`과 `set()`을 호출한다.

코틀린에서 배열은 불변한다. 코틀린은 `Array<String>`을 `Array<Any>`에 할당하는 것을 막아 런타임 실패를 방지한다(그러나 Array<out Any>는 사용 가능하다. [*타입 프로젝션](https://kotlinlang.org/docs/reference/generics.html#type-projections)을 보라).

### 기본 타입 배열

코틀린은 기본 타입 배열을 박싱 비용 없이 표현하게 해주는 특화된 클래스를 제공한다: `ByteArray`, `ShortArray`, `IntArray` 등등. 이 클래스들은 `Array` 클래스와 상속 관계가 아니지만 동일한 메서드와 프로퍼티 묶음을 가진다. 또한 각각 연관되는 팩터리 함수를 가지고 있다.

```kotlin
val x: IntArray = intArray(1, 2, 3)
x[0] = x[1] + x[2]
```

```kotlin
// [0, 0, 0, 0, 0] 값의 크기 5 정수 배열 
val arr = IntArray(5)

// 예. 상수값으로 배열값을 초기화하기
val arr = IntArray(5) {42}

// 예. 람다를 사용해 배열값을 초기화하기
// [0, 1, 2, 3, 4] 값의 크기 5 정수 배열 (인덱스 값으로 초기화된다)
val arr = IntArray(5) { it * 1 }
```

## 무부호 정수

> 무부호(unsigned) 타입은 코틀린 1.3 이후 버전에서 가능하며 현재 실험적인(experimental) 기능으로 추가되어 있다. [아래](#무부호-정수의-실험적-상황)에서 상세한 사항을 확인하라.

코틀린의 무부호 정수형은 다음과 같다:

- `kotlin.UByte`: 0부터 255 범위의 무부호 8비트 정수
- `kotlin.UShort`: 0부터 65535 범위의 무부호 16비트 정수
- `kotliin.UInt`: 0부터 2<sup>32</sup>-1 범위의 무부호 32비트 정수
- `kotlin.ULong`: 0부터 2<sup>64</sup>-1 범위의 무부호 64비트 정수

무부호 타입은 대응되는 부호 타입에서 사용되는 대부분의 연산을 지원한다.

> 무부호 타입에서 대응되는 부호 타입으로의 변환(그 반대 역시)은 이진법상 호환되지 않는 변환임을 유의하라.

무부호 타입은 다른 실험적(experimental) 기능인 [*인라인 클래스](https://kotlinlang.org/docs/reference/inline-classes.html)를 사용하여 구현되었다.

### 특화된 클래스

기본 타입과 마찬가지로, 각각의 무부호 타입은 그에 대응하는 배열 타입을 가진다.

- `kotlin.UByteArray`: unsigned byte 배열
- `kotlin.UShortArray`: unsigned short 배열
- `kotlin.UIntArray`: unsigned int 배열
- `kotlin.ULongArray`: unsigned long 배열

이들은 부호 정수 배열과 마찬가지로 박싱 비용 없이 `Array` 클래스와 비슷한 API를 제공한다.

또한 `UInt`와 `ULong`의 [*범위와 수열](https://kotlinlang.org/docs/reference/ranges.html)은 `kotlin.ranges.UIntRange`, `kotlin.ranges.UIntProgression`, `kotlin.ranges.ULongRange`, `kotlin.ranges.ULongProgression` 클래스를 통해 지원된다.

### 리터럴

무부호 정수를 쉽게 사용하기 위해, 코틀린은 정수 리터럴에 특정 무부호 타입을 의미하는 접미사로 꼬리표를 달 수 있는 기능을 제공한다(Float/Long에 사용되는 것과 유사하게).

- 접미사 `u`와 `U`는 리터럴을 무부호로 표시한다. 정확한 타입은 예상 타입을 기반으로 결정된다. 만약 예상 타입을 제공하지 않는다면 리터럴 크기에 따라 `UInt`또는 `ULong`이 선택된다.

```kotlin
val b: UByte = 1u   // UByte, 예상 타입 제공됨
val s: UShort = 1u   // UShort, 예상 타입 제공됨
val l: ULong = 1u   // ULong, 예상 타입 제공됨

val a1 = 42u    // UInt: 예상 타입 미제공, 상수가 UInt에 적합함
val a2 = 0xFFFF_FFFF_FFFu   // ULong: 예상 타입 미제공, 상수가 UInt에 적합하지 않음
```

- 접미사 `uL`과 `UL`은 명시적으로 리터럴을 unsigned long으로 표시한다.

### 무부호 정수의 실험적 상황

무부호 타입의 설계는 실험적이다. 즉 그 뜻은 이 기능이 빠르게 수정되고 있으며 호환성은 보장되지 않는다는 의미이다. 코틀린 1.3 이상에서 무부호 연산을 사용할 때는 이 기능이 실험적이라는 경고가 출력될 것이다. 경고를 제거하기 위해서는 무부호 타입의 실험적 사용을 위해 사전 동의를 거쳐야 한다.

무부호 타입을 위한 사전 동의에는 API를 실험적(experimental)로 표시하거나, 표시하지 않고 하는 두 가지 방법이 있다.

- 실험성을 전파하기 위해, 무부호 정수를 사용하는 선언을 `@ExperimentalUnsignedTypes` 어노테이션을 달거나 컴파일러에 `-Xexperimental=kotlin.ExperimentalUnsignedTypes`를 넘긴다(후자는 컴파일되는 모듈의 모든 선언을 실험적으로 표시하게 되는 것을 유의하라).
- 실험성을 전파하지 않는 사전 동의를 위해서, 선언dp `@UseExperimental(ExperimentalUnsignedTypes::class)` 어노테이션을 달거나 `-Xuse-experimental=kotlin.ExperimentalUnsignedTypes`를 넘긴다.

클라이언트가 API를 사용할 때 명시적으로 사전 동의를 수행하게 할지의 여부는 사용자에게 달려있지만, 무부호 타입은 실험적 기능이므로 사용하는 API가 언어의 변동으로 갑자기 깨질 수 있음을 명심하라.

추가적으로 기술적인 세부사항을 확인하기 위해서는 실험적 API [KEEP](https://github.com/Kotlin/KEEP/blob/master/proposals/experimental.md)을 확인하라.

### 추가적 논의

기술적 세부사항과 추가적인 논의사항들을 확인하기 위해 [무부호 타입을 위한 언어 제안](https://github.com/Kotlin/KEEP/blob/master/proposals/unsigned-types.md)을 참고하라.


## 문자열

문자열은 `String`타입으로 표현된다. 문자열은 불변이다. 문자열의 요소들은 인덱스 연산으로 접근할 수 있는 문자들이다: `s[i]`. 문자열은 `for`문으로 순회할 수 있다:
```kotlin
for (c in str) {
    println(c)
}
```

`+` 연산자를 사용하여 문자열을 연결할 수 있다. 이 방식으로 첫 요소가 문자열인 경우 문자열과 다른 타입의 값 역시 연결할 수 있다:
```kotlin
val s = "abc" + 1
println(s + "def")
```

대부분의 경우에 문자열 연결보다는 [문자열 템플릿](#문자열-템플릿)이나 순수한 문자열의 사용이 권장된다는 것을 유념하라.

### 문자열 리터럴

코틀린에는 두 종류의 문자열 리터럴이 있다: 이스케이프 문자를 포함할 수 있는 이스케이프 문자열과, 줄바꿈과 임의의 본문을 포함하는 순수 문자열이다. 다음은 이스케이프 문자열의 예시이다:
```kotlin
val s = "Hello, world!\n"
```

역슬래시를 사용하여 관례대로 줄바꿈이 적용된다. 위의 [문자](#문자) 섹션에서 지원되는 이스케이프 시퀀스 목록을 확인할 수 있다.

순수 문자열은 세쌍 따옴표(`"""`)를 경계로 사용하는데, 이스케이프 문자를 포함하지 않고 줄바꿈과 다른 어떤 문자라도 포함시킬 수 있다.
```kotlin
val text = """
    for (c in "foo")
        print(c)
"""
```

`trimMargin()` 함수를 사용해 선두의 빈 칸을 제거할 수 있다.
```kotlin
val text = """
    |나에게 말하라, 그러면 잊을 것이고,
    |나에게 가르치라, 그러면 기억할 것이며,
    |나를 참여시켜라, 그러면 배울 것이다.
    |(벤자민 프랭클린)
    """.trimMargin()
```

기본 설정으로 `|`는 여백 접두사로 쓰이지만, `trimMarign(">")`과 같이 다른 문자를 사용하고 파라미터로 넘길 수 있다로

### 문자열 템플릿

문자열 리터럴은 템플린 표현, 즉 계산된 결과값이 문자열에 연결되는 코드 조각을 포함할 수 있다. 템플릿 표현은 달러 문자($)로 시작되며 간단한 이름으로 구성된다:
```kotlin
val i = 10
println("i = $i") // "i = 10" 출력
```

혹은 중괄호 안의 임의의 표현식으로 구성할 수 있다.
```kotlin
val s = "abc"
println("$s.length는 ${s.length}") // "abc.length는 3" 출력
```

템플릿은 순수 문자열과 이스케이프 문자열 둘 모두의 안에서 사용될 수 있다. 만약 역슬래시 이스케이프 문자를 지원하지 않는 순수 문자열에서 리터럴 `$` 문자를 사용하고 싶다면 다음과 같이 사용하면 된다.
```kotlin
val price = """
${'$'}9.99
"""