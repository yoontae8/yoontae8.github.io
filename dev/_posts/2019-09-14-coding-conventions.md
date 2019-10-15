---
layout: post
title: 시작하기 - 3. 코딩 규약
categories: dev
tags: kotlin
---

이번 장은 코드를 작성하는 데 지켜야 할 서식이나 좋은 변수 이름을 사용하는 법 등 코딩 규약에 대해 다룬다. 어떻게 보면 당연하게 생각해 왔던 부분이며, 언어 자체에 대한 내용은 아닐 수 있지만 협업이나 가독성 등에 있어서 무엇보다 중요할 수 있다. 내용이 길고 언어에 대한 이해를 필요로 하는 부분도 있으니 2장을 먼저 공부하고 돌아와도 좋겠다.

---

[원문: Getting Started - Coding Conventions](https://kotlinlang.org/docs/reference/coding-conventions.html)

{% include_relative kotlin-reference-index.md %}

## 목차
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

### 스타일 가이드 적용하기

이 스타일 가이드대로 인텔리제이 formatter를 설정하기 위해서는 1.2.20 이상의 코틀린 플러그인을 설치해야 한다. **Setting** \| **Editor** \| **Code Style** \| **Kotlin** 으로 가서 오른쪽 위 구석탱이에 있는 **Set from..** 링크를 선택하고 메뉴에서 **Predefined style** \| **Kotlin style guide**를 선택한다.

코드가 스타일 가이드를 따르고 있다는 것을 확인하려면, inspection 설정으로 가서 **Kotlin** \| **Style issues** \| **File is not formatted according to project settings** 검사를 활성화한다. 스타일 가이드에서 설명하고 있는 다른 사항들(명명법에 대한 규약과 같은)을 확인하기 위한 추가적인 검사는 기본적으로 활성화되어 있다.

## 소스코드 구성하기

### 디렉토리 구조

순수한 코틀린 프로젝트에서 권장되는 디렉토리 구조는 공통의 루트 패키지가 생략된 패키지 구조를 따른다. 예를 들어, 프로젝트의 모든 코드가 `org.example.kotlin` 패키지와 그 하위 패키지 안에 존재한다면 `org.example.kotlin` 패키지의 파일들은 소스 루트 디렉토리 아래 위치하여야 하며, `org.example.kotlin.network.socket`의 파일들은 소스 루트 파일의 하위 디렉토리인 `network/socket`아래 위치해야 한다.

> JVM에서: 코틀린과 자바가 혼용되는 프로젝트에서 코틀린 소스 파일들은 자바 소스 파일과 같은 소스 루트 아래 존재해야 하며 같은 디렉토리 구조를 가져야 한다: 각각의 파일은 각각 패키지 선언에 일치하는 디렉토리에 존재해야 한다.

### 소스 파일 이름

코틀린 파일에 하나의 클래스만 있을 경우(다른 최상위 선언들과 함께), 파일의 이름은 .kt 확장자를 가지며 클래스의 이름과 동일해야 한다. 파일이 여러 클래스를 가지고 있거나 최상위 선언들만 있을 경우 파일의 내용에 걸맞는 이름으로 지정한다. Pascal Case를 사용하라 (예. ProcessDelcarations.kt)

파일의 이름은 파일에 작성된 코드의 동작을 설명할 수 있어야 한다. 그러므로, 파일 이름에 "Util" 같이 의미없는 단어를 사용하지 않는다.

### 소스 파일 구성

여러 선언들(클래스, 최상위 함수나 프로퍼티 등)을 하나의 코틀린 소스 파일에 작성하는 것은 해당 선언들이 의미상 밀접하게 연관되어 있고 파일의 크기가 너무 크지 않을 경우(수백 라인을 넘지 않을 정도)에만 권장된다.

예외적으로, 한 클래스를 사용하는 모든 *클라이언트를 위한 확장 함수들을 정의할 경우에는 그 클래스가 정의된 파일에 같이 두어야 한다. 만일 특정 클라이언트만을 위한 확장 함수를 정의한다면 그 클라이언트의 코드 옆에 두어라. 그저 "Foo의 모든 확장"을 담기 위한 파일을 생성하면 안 된다.
> 클라이언트: 다른 개체의 서비스를 요청하는 개체(any entity that requests a service from another entity) -> 즉 위에서는 그 클래스의 기능을 사용하는 요소를 말한다.

### 클래스 배치

보통의 경우, 클래스의 내용은 다음과 같은 순서를 따른다.

- 프로퍼티 선언과 초기화 블록
- 부가적인 생성자들
- 함수 선언
- 동반 객체

함수 선언을 알파벳 순서나 가시성 기준으로 배치하지 말고 일반 함수와 확장 함수를 분리하지 말아라. 대신에 누군가가 클래스를 처음부터 끝까지 읽을 때 어떤 일을 하는지 이해할 수 있도록 관련된 것들을 함께 놓아라. 순서를 정하고(높은 수준 먼저, 혹은 반대로) 그대로 유지하라.

중첩 클래스는 해당 클래스를 사용하는 코드 옆에 놓아라. 만약 클래스가 외부에서만 사용되고 내부에서 참조되지 않는다면 마지막, 즉 동반 객체 다음에 놓아라.

### 인터페이스 구현시 배치

인터페이스를 구현할 경우, 구현하는 항목들의 순서를 인터페이스의 순서와 동일하게 유지하라(만약 필요하다면 구현에 사용한 추가적인 private 함수와 함께).

### 오버로딩시 배치

클래스에서 오버로딩된 항목들은 항상 함께 배치하라.

## 명명 규칙

코틀린에서 패키지와 클래스에 대한 명명 규칙은 간단하다.

* 패키지의 이름은 항상 소문자를 사용하며 언더스코어(_)를 사용하지 말아야 한다(org.example.project). 여러 단어를 사용한 이름은 일반적으로 권장되지 않지만, 정말 필요한 경우 간단하게 두 단어를 연결하거나 카멜 표기법을 사용한다(org.example.myProject).
* 클래스와 오브젝트(object)의 이름은 파스칼 표기법을 사용한다:
```kotlin
open class DeclarationProcessor { ... }

object EmptyDeclarationProcessor : DeclarationProcessor { ... }
```

### 함수 이름

메서드와 프로퍼티, 로컬 변수의 이름은 카멜 표키법을 사용하며 언더스코어(_)는 피해야 한다:
```kotlin
fun processDeclarations() { ... }

var declarationCount = ...

예외: 인스턴스를 생성하기 위한 팩토리 메서드는 생성하는 클래스와 같은 이름을 가질 수 있다:
```kotlin
abstract class Foo { ... }

class FooImpl : foo { ... }

fun Foo(): Foo { return FooImpl(...) }
```

#### 테스트 메서드 이름

테스트에서는 역따옴표(`)를 사용하여 스페이스를 포함한 이름을 사용할 수 있다.(현재 안드로이드 런타임에서는 지원하지 않음) 언더스코어(_) 또한 사용 가능하다.
```kotlin
class MyTestCase {
    @Test fun `ensure everything works`() { ... }

    @Test fun ensureEverythingWorks_onAndroid() { ... }
}
```

### 프로퍼티 이름

상수(`const` 프로퍼티 혹은 변경 불가능한 값을 가지며 get 메서드를 구현하지 않은 최상위/object `val` 프로퍼티)의 이름은 대문자를 사용하며 단어를 언더스코어(_)로 연결한다.
```kotlin
const val MAX_COUNT = 8
val USER_NAME_FIELD = "UserName"
```

행위나 변경 가능한 데이터를 가진 최상위/object 프로퍼티의 이름은 일반적인 카멜 표기법을 사용한다.
```kotlin
val mutableCollection: MutableSet<String> = HashSet()

싱글턴 객체를 참조하는 프로퍼티의 이름은 `object` 선언과 같은 명명법을 사용할 수 있다.
```kotlin
val PersonComparator: Comparator<Person> = ...
```

이넘 상수의 이름은 용도에 따라 대문자와 언더스코어(_)를 사용한 이름(`enum class Color { RED, GREEN } )과 일반적인 파스칼 표기법 모두 사용 가능하다.

#### backing 프로퍼티의 이름

클래스 안에 개념상으로 같은 두 개의 프로퍼티가 있는데 하나는 공용 API의 일부이고 하나는 내부적으로 사용된다면, private 프로퍼티의 이름 앞에 언더스코어(_)를 사용하라.
```kotlin
class C {
    private val _elementList = MutableListOf<Element>()

    val elementList: List<Element>
}
```

### 좋은 이름 정하기

클래스의 이름에는 클래스가 *무엇인지* 설명하는 명사 혹은 명사구를 사용한다: `List`, `PersonReader`.

메서드의 이름에는 메서드가 *무엇을 하는지* 설명하는 동사 혹은 동사구를 사용한: `close`, `readPersons`. 또한 이름은 객체를 변경하는지, 혹은 새 객체를 반환하는지 나타내야 한다. 예를 들어 `sort`는 컬렉션 그 자체를 정렬하지만, `sorted`는 컬렉션의 정렬된 복사본을 반환한다.

이름은 개체의 목적이 무엇인지 명확하게 나타내야 하므로, 의미 없는 단어(`Manager`, `Wrapper`)를 사용하는 것을 피하는 것이 좋다.

두음문자를 이름의 일부로 사용할 경우, 두 글자는 대문자로 쓰고(`IOStream`) 세 글자 이상은 첫 문자만 대문자로 작성하라(`XmlFormatter`, `HttpInputStream`).

## 서식(formatting)

들여쓰기 시 tab을 사용하지 말고 4개의 스페이스를 사용하라.

중괄호의 경우, 여는 괄호는 시작되는 줄의 끝에 두고, 닫는 괄호는 블록의 마지막 아래 새로운 줄에 첫 줄과 들여쓰기를 맞춰 배치한다.
```kotlin
if (elements != null) {
    for (element in elements) {
        // ...
    }
}
```
(참고: 코틀린에서는 세미콜론(;)을 생략할 수 있기 때문에 줄 바꿈이 매우 중요하다. 언어가 자바 스타일의 괄호를 상정하고 디자인되었기 때문에 다른 서식 스타일을 사용하려고 할 경우 예상치 못한 현상이 발생할 수 있다.)

### 줄에서의 여백

2항 연산자 앞뒤로는 빈 칸을 두어야 한다 (`a + b`). 예외적으로, "range to" 연산자 주변에는 빈 칸을 두지 말아라(`0..1`).

단항 연산자 앞뒤로는 빈 칸을 두면 안 된다 (`a++`)

흐름 제어 키워드(`if`, `when`, `for` 와 `while`)와 따라오는 여는 괄호 사이에는 빈 칸을 두어야 한다.
```kotlin
while (a > 0) {
    ...
}
```

주 생성자 선언, 메서드 선언이나 메서드 호출의 여는 괄호 앞에는 빈 칸을 두면 안 된다.
```kotlin
class A(val x: Int)

fun foo(x: Int) { ... }

fun bar() {
    foo(1)
}
```

`(`, `[` 다음이나 `]`, `)` 앞에 빈 칸을 두지 말아라.

`.`나 `?.` 앞뒤로 빈 칸을 두지 말아라: `foo.bar().filter { it > 2 }.joinToString()`, `foo?.bar()`

`//` 다음에는 빈 칸을 두어라: `// this is a comment`

타입 파라미터를 명시하기 위한 꺾쇠괄호 앞뒤로는 빈 칸을 두지 말아라: `class Map<K, V> { ... }`

`::` 앞뒤로 빈 칸을 두지 말아라: `Foo::class`, `String::length`

nullable 타입을 표시하기 위한 ? 앞에 빈 칸을 두지 말아라: `String?`

일반 규칙으로, 한 줄 내에서 정렬하려 하지 말라. 식별자의 이름을 다른 길이의 이름으로 변경하는 것은 선언이나 사용의 서식에 영향을 주지 않는다.

### 콜론

다음과 같은 경우, `:` 앞에 빈 칸을 두어라.
* 타입과 수퍼타입을 구분하기 위해서 사용되었을 때;
* 상위 클래스의 생성자나 같은 클래스의 다른 생성자에 위임할 때;
* object 키워드 다음에.

정의와 그 타입을 구분할 경우 `:` 앞에 빈 칸을 두지 말아라.

`:` 다음에는 항상 빈 칸을 두어라.
```kotlin
abstract class Foo<out T : Any> : IFoo {
    abstract fun foo(a: Int): T
}

class FooImpl : Foo() {
    constructor(x: String) : this(x) { ... }

    val x = object : IFoo { ... }
}
```

### 클래스 헤더 서식

주 생성자 파라미터 개수가 적은 클래스는 한 줄로 작성한다.
```kotlin
class Person(id: Int, name: String)
```

더 긴 헤더를 가진 클래스들은 각가의 주 생성자 파라미터 하나의 라인을 차지하도록 작성해야 한다. 또한 닫는 괄호는 다음 줄에 자리해야 한다. 상속을 사용한다면 상위 클래스 호출이나 구현된 인터페이스 목록은 괄호와 같은 줄에 위치해야 한다.
```kotlin
class Person (
    id: Int,
    name: String,
    surname: String
) : Human(id, name) { ... }
```

여러 개의 인터페이스를 구현할 경우, 상위 클래스 호출이 처음에 오고 각각의 인터페이스가 다른 줄에 위치해야 한다.
```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name),
    KotlinMaker { ... }
```

긴 수퍼타입 목록을 가진 클래스는 모든 수퍼타입 이름을 콜론 아래 새 줄에 횡적으로 열을 맞춰 표시한다.
```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLonogClass>(),
    SomeOtherInterface,
    AndAnotherOne {

    fun foo() { ... }
}
```

클래스 해더가 긴 경우 몸체와 분명히 구분하기 위해서, 클래스 해더 아래에 빈 줄을 넣거나 혹은 여는 중괄호를 다음 줄에 배치하라.
```kotlin
class MyFavouriteVeryLongClassHolder:
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterfaces,
    AndAnotherOne
{
    fun foo() { ... }
}
```

생성자 파라미터를 위해 일반 들여쓰기(공백 4개)를 사용하라.
> 이유: 이렇게 함으로서 주 생성자에 선언된 프로퍼티와 클래스의 몸체에 선언된 프로퍼티를 횡적으로 같은 위치에 맞출 수 있다.

### 제어자

선언이 여러 개의 제어자를 사용한다면 항상 다음과 같은 순서로 배치하라.
```kotlin
public / protected / private / internal
expect / actual
final / open / abstract / sealed / const
external
override
lateinit
tailrec
vararg
suspend
inner
enum / annotation
companion
inline
infix
operator
data
```

모든 어노테이션을 제어자 이전에 놓아라.
```kotlin
@Named("Foo")
private val foo: Foo
```

라이브러리를 위해 작업하는 것이 아니라면, 명시할 필요가 없는 제어자를 생략하라(예. `public`)

### 어노테이션 서식

어노테이션은 적용되는 선언 이전의 별개 줄에 선언과 같은 만큼 들여쓰기해 배치한다.
```kotlin
@Target(AnnotationTarget.PROPERTY)
annotation class JsonExclude
```

인자 없는 어노테이션들은 같은 줄에 배치할 수 있다.
```kotlin
@JsonExclude @JvmField
var x: String
```

인자 없는 하나의 어노테이션은 적용되는 선언과 같은 줄에 배치할 수 있다.
```kotlin
@Test fun foo() { ... }

### 파일 어노테이션

파일 어노테이션은 파일 주석(있다면)의 다음에, `package` 구문 이전에 배치되며, 패키지가 아닌 파일을 대상으로 한다는 점을 강조하기 위해 `package`와 빈 줄로 구분된다.
```kotlin
/** License, copyright and whatever */
@File:JvmName("FooBar")

package foo.bar
```

### 함수 서식

함수명과 파라미터가 한 줄에 들어가지 않을 경우, 다음과 같이 작성하라.
```kotlin
fun longMethodName(
    argument: ArgumentType = defaultValue,
    arguement2: AnotherArguementType
): ReturnType {
    // body
}
```

함수 파라미터에는 일반 들여쓰기(공백 4개)를 사용하라.
> 이유: 생성자 파라미터와 일관되게 정렬할 수 있도록.

함수 몸체가 하나의 표현식만을 가질 경우 구문보다는 표현식 몸통을 사용해 나타내라.
```kotlin
fun foo(): Int {    // 좋은 예
    return 1
}

fun foo() = 1       // 나쁜 예
```

### 표현식 몸체 서식

함수의 선언부와 몸체를 같은 줄에 둘 수 없다면, `=`를 첫째 줄에 두라. 표현식 몸통을 공백 4개만큼 들여쓰기하라.
```kotlin
fun f(x: String) = 
    x.length
```

### 프로퍼티 서식

간단한 읽기 전용 프로퍼티들은 한 줄로 작성하는 것을 고려하라.
```kotlin
val isEmpty: Boolean get() = size == 0
```

이보다 복잡한 프로퍼티들은 항상 `get`과 `set` 키워드를 별도의 줄에 배치하라.
```kotlin
val foo: String
    get() { ... }
```

긴 초기화 구문을 가진 프로퍼티의 경우 등호 다음에 줄을 나누고 초기화 구문을 공백 4개만큼 들여쓰기해 작성하라.
```kotlin
private val defaultCharset: Charset? = 
    EncodingRegistry.getInstance().getDefaultCharsetForPropertiesFiles(file)
```

### 흐름 제어 구문 서식

`if`나 `when` 구문의 조건절이 여러 줄로 작성되어 있을 경우 항상 구문을 중괄호로 감싸라. 각각의 조건들을 구문의 시작부분에 맞춰 공백 4개만큼 들여쓰기하라. 조건절의 닫는 괄호를 다음줄의 여는 중괄호와 함께 두어라.
```kotlin
if (!component.isSyncing &&
    !hasAnyKotlinRuntimeInScope(module)
) {
    return createKotlinNotConfigurePanel(module)
}
```
> 이유: 깔끔한 줄맞춤과, 조건과 구문 몸체의 분명한 구분을 위해서

`else`, `catch`, `finally` 키워드, 그리고 do/while 반복문의 `while` 키워드는 앞의 중괄호와 같은 줄에 배치하라.
```kotlin
if (condition) {
    // 몸체
} else {
    // else 부분
}

try {
    // 몸체
} finally {
    // 정리
}
```

`when` 구문에서, 분기 하나가 여러 줄로 이루어져 있을 경우 다음 분기는 한 줄을 띄고 작성하는 것을 고려하라.
```kotlin
private fun parsePropertyValue(propName: String, token: Token) {
    when (token) {
        is Token.ValueToken ->
            callback.visitVaue(propName, token.value)
        
        Token.LBRACE -> { // ...
        }
    }
}
```

짧은 분기는 괄호 없이 같은 줄에 배치하라.
```kotlin
when (foo) {
    true -> bar()  // 좋은 예
    false -> { baz() } // 나쁜 예
}
```

### 메서드 호출 서식

전달 인자 수가 많을 경우, 여는 괄호 다음 줄을 바꾸고 공백 4개만큼 들여쓰기해 작성하라. 관련이 깊은 인자를 같은 줄에 배치하라.
```kotlin
drawSquare(
    x = 10, y = 10,
    width = 100, height = 100,
    fill = true
)
```

### 연쇄 호출 줄 바꿈

연쇄 호출에서 줄 바꿈을 적용할 때, `.` 나 `?.` 문자는 줄을 분리하고 일관되게 들여쓰기하여 작성하라.
```kotlin
val anchor = owner
    ?.firstChild!!
    .siblings(forward = true)
    .dropWhile { it is PsiComment || it is PsiWhiteSpace }
```
연쇄의 첫 호출은 새로운 줄에서 시작해야 하지만, 자연스러운 형태라면 생략해도 된다.

### 람다 서식

람다 표현식에서 중괄호, 그리고 몸체와 파라미터를 구분하는 화살표 좌우에는 빈 칸을 두어야 한다. 호출에 하나의 람다만 존재할 경우 가능한 한 괄호 바깥으로 빼내어 작성해야 한다.
```kotlin
list.filter { it > 10 }
```

람다를 위한 라벨을 지정할 경우, 라벨과 시작하는 중괄호 사이에는 빈 칸을 두지 말아라.
```kotlin
fun foo() {
    ints.forEach lit@{
        // ...
    }
}
```

여러 줄로 작성된 람다에서 파라미터 이름을 선언할 경우, 이름을 화살표와 개행으로 이어지는 첫 번째 줄에 두어라.
```kotlin
appendCommaSeparated(properties) { prop ->
    val propertyValue = prop.get(obj)   // ...
}
```

파라미터 목록이 한 줄에 작성하기에는 너무 길 경우, 화살표를 다른 줄에 작성하라.
```kotlin
foo {
    context: Context,
    environment: Env
    ->
    context.configureEnv(environment)
}
```

## 문서화 주석

문서화를 위한 주석이 길 경우 시작하는 `/**`를 별개의 줄에 두고 각각의 이어지는 줄을 별표(`*`)로 시작하라.
```kotlin
/**
 * 여러 줄에 걸친
 * 문서화 주석
 */
```

짧은 주석은 한 줄에 표시할 수 있다.
```kotlin
/** 짧은 문서화 주석 */
```

일반적인 경우 `@param`과 `@return` 태그 사용을 피하라. 대신, 파라미터의 설명과 리턴 값을 문서 주석에 직접적으로 포함시키고 언급될 때마다 파라미터로의 링크를 추가하라. 오직 중심 본문의 흐름에 맞지 않는 긴 설명이 필요할 때만 `@param`과 `@return`을 사용하라.
```kotlin
// 피해야 할 예

/**
 * 주어진 숫자의 절대값을 반환한다.
 * @param number 절대값을 반환할 숫자
 * @return 절대값
 */
fun abs(number: Int) { /*...*/ }

// 올바른 예

/**
 * 주어진 [number]의 절대값을 반환한다.
 */
fun abs(number: Int) { /*...*/ }
```


## 과다한 생성자를 피하라

보통의 경우, 코틀린에서의 특정한 구문 구성이 IDE에 의해 선택적이며 불필요하다고 표시될 경우 코드에서 생략해야 한다. 단지 명확성을 위해 중요하지 않은 구문 요소를 남겨놓지 말아라.

### Unit

함수가 Unit을 반환할 경우, 반환 타입은 생략되어야 한다.
```kotlin
fun foo() { // ": Unit" 은 생략됨

}
```

### 세미콜론

가능한 한 세미콜론을 생략하라.

### 문자열 템플릿

문자열 템플릿에 간단한 변수를 삽입할 때에는 중괄호를 사용하지 말아라. 긴 표현식에서만 중괄호를 사용하라.
```kotlin
println("$name의 아이는 ${children.size}명이다.")
```

## 언어의 특성을 살리기

### 불변성

가변 데이터보다 불변 데이터를 주로 사용하라. 초기화 후 변경되지 않는 지연 변수와 프로퍼티의 선언은 항상 `var`이 아닌 `val`을 사용하라.

변경되지 않는 컬렉션을 선언할 때는 항상 불변 컬렉션 인터페이스(`Collection`, `List`, `Set`, `Map`)를 사용하라. 컬렉션 인스턴스를 생성하기 위해 팩터리 함수를 사용한다면, 가능한 경우 항상 불변 컬렉션 타입을 반환하는 함수를 사용하라:
```kotlin
// 나쁜 예: 변경되지 않을 값을 위해 변경 가능한 컬렉션을 사용
fun validateValue(actualValue: String, allowedValues: HashSet<String>) { ... }

// 좋은 예: 불변 컬렉션 사용
fun validateValue(actualValue: String, allowedValues: Set<String>) { ... }

// 나쁜 예: arrayListOf()는 변경 가능한 컬렉션 타입인 ArrayList<T>를 반환한다.
val allowedValues = arrayListOf("a", "b", "c")

// 좋은 예: listOf()는 List<T>를 반환한다.
val allowedValues = listOf("a", "b", "c")
```


### 기본 파라미터

함수를 오버로딩하여 사용하기보다는 기본 파라미터를 사용하여 선언하라.
```kotin
// 나쁜 예
fun foo() = foo("a")
fun foo(a: String) { /*...*/ }

// 좋은 예
fun foo(a: String = "a") { /*...*/ }
```

### 타입 별칭

코드 전체에서 함수형 타입이나 타입 파라미터가 있는 타입이 여러 번 사용될 경우, 타입 별칭을 사용하라.
```kotlin
typealias MouseClickHandler = (Any, MouseEvent) -> Unit
typealias PersonIndex = Map<String, Person)
```

### 람다 파라미터

짧고 중첩되지 않은 람다의 경우, 파라미터를 명시적으로 선언하는 것보다 `it`을 사용하는 것이 권장된다. 중첩된 람다가 파라미터를 가지고 있을 경우 파라미터는 항상 명시적으로 선언되어야 한다.

### 람다에서의 반환

람다에서 반환에 여러 라벨을 붙이는 것을 피하라. 람다를 재구성하여 한 지점에서 끝나게 하는 것을 고려하라. 그 방법이 불가능하거나 분명하지 않을 경우 람다를 익명 함수로 변경하는 것을 고려하라.

### 지명 인자

메서드가 여러 개의 같은 기본 타입 파라미터 또는 `Boolean` 타입의 파라미터를 가질 때, 문맥으로부터 모든 파라미터의 의미를 완벽히 판단할 수 있는 경우가 아니면 항상 지명 인자 구문을 사용하라. 
```kotlin
drawSquare(x = 10, y = 10, width = 100, height = 100, fill = true)
```

### 조건문 사용

표현식 형태의 `try`, `if`, `when`을 주로 사용하라. 예시:
```kotlin
return if (x) foo() else bar()

return when(x) {
    0 -> "zero"
    else -> "nonzero"
}
```
아래보다 위 예를 사용하라.
```kotlin
if (x)
    return foo()
else
    return bar()

when(x) {
    0 -> return "zero"
    else -> return "nonzero"
}
```

### if vs when
두 개의 조건이 있는 경우 `when`보다 `if`를 사용하라. 즉,
```kotlin
when (x) {
    null -> // ...
    else -> // ...
}
```
보다
`if (x == null) ... else ...`를 사용하라.

세 개 이상의 조건이 있을 경우 `when`을 사용하라.

### 조건문에서 null일 수 있는 Boolean 값 사용하기

조건문에서 null일 수 있는 `Boolean` 값을 사용할 경우, `if (value == true)`나 `if (value == false)`로 확인하라.

### 반복문 사용하기

반복문에서는 `filter`, `map` 등의 고차함수*를 사용하라. 예외: `forEach` (`forEach`의 수신자가 nullable이거나 `forEach`가 긴 연쇄 호출의 일부로 사용되었을 경우 외에는 `forEach` 대신 일반 `for`를 사용하라).

여러 고차함수를 사용하는 복잡한 표현식과 반복문 중에 선택해야 할 경우, 각각의 경우에 대한 연산의 비용을 이해하고 성능 검토를 염두에 두라.
> 고차함수: 하나 이상의 함수를 인자로 받거나 함수를 결과로 반환하는 함수

### 범위에 대한 반복

열린 범위를 순회하기 위해 `until` 함수를 사용하라.
```kotlin
for (i in 0..n - 1) { /*...*/ } // 나쁜 예
for (i in 0 until n) { /*...*/ } // 좋은 예
```

### 문자열 사용하기

문자열을 연결하려면 문자열 템플릿을 활용하라.

일반 문자열 리터럴에 `\n` 이스케이프 문자를 삽입하는 것보다, 여러줄 문자열을 사용하라.

여러줄 문자열의 들여쓰기를 유지하기 위해서, 결과 문자열이 내부 들여쓰기를 필요로 하지 않을 경우에는 `trimIndent`를, 내부 들여쓰기를 필요로 할 경우에는 `trimMargin`을 사용하라.
```kotlin
assertEquals(
    """
    Foo
    Bar
    """.trimIndent(),
    value
)

avl a = """if(a > 1) {
          |     return a
          |}""".trimMargin()
```

### 함수 vs 프로퍼티

특정한 경우에 인자가 없는 함수는 읽기 전용 프로퍼티와 교환할 수 있다. 의미는 비슷하지만, 어떤 것을 언제 사용할지에 대한 문체상의 관례가 있다.

기반 알고리즘이 다음과 같은 경우 함수보다 프로퍼티를 선택하라:
- 예외를 던지지 않을 때
- 계산이 간단할 때(혹은 첫 번째 실행시 캐싱할 때)
- 객체 상태가 변경되지 않을 경우 호출해도 같은 값을 반환할 경우

### 확장 함수 사용하기

확장 함수를 마음껏 사용하라. 객체에 주로 작동하는 함수가 있을 경우, 객체를 수신하는 확장 함수로 작성하는 것을 고려하라. API 오염을 최소하기 위하여 가능한 한 확장 함수의 접근을 제한하라. 필요에 따라 지역 확장함수, 멤버 확장함수, 혹은 최상위 확장함수를 private으로 사용하라.

### 중위 함수 사용하기

오직 함수가 비슷한 역할을 하는 두 객체에 대해 작동할 때만 중위 함수를 선언하라. 좋은 예시: `and`, `to`, `zip`. 나쁜 예: `add`.

수신 객체를 변경할 경우 메서드를 중위로 선언하지 말아라.

### 팩터리 메서드

클래스를 위한 팩터리 함수를 선언할 경우, 클래스와 같은 이름을 사용하지 말라. 왜 팩터리 함수의 동작이 특별한지 분명히 나타내는 구분된 이름을 사용하라. 정말 특별한 의미가 없을 경우에만 클래스와 같은 이름을 사용하라.

예시:
```kotlin
class Point(val x: Double, val y: Double) {
    companion object {
        fun fromPolar(angle: Double, radius: Double) = Point(...)
    }
}
```
객체에 상위 클래스의 생성자를 호출하지 않으며 기본 인자값을 사용하여 하나의 생성자로 줄일 수 없는 여러 개의 오버로드된 생성자들이 있을 경우, 오버로드된 생성자를 팩터리 함수로 대체하는 것을 고려하라.

### 플랫폼 타입

플랫폼 타입의 표현식을 반환하는 public 함수/메서드는 반드시 그 코틀린 타입을 명시적으로 선언해야 한다.
```kotlin
fun apiCall(): String = MyJavaApi.getProperty("name")
```

플랫폼 타입의 표현식으로 초기화된 프로퍼티(패키지 레벨이나 클래스 레벨의)는 그 코틀린 타입을 명시적으로 선언해야 한다.
```kotlin
class Person {
    val name: String = MyJavaApi.getProperty("name")
}

플랫폼 타입의 표현식으로 초기화된 지역 값의 타입 선언은 선택사항이다.
```kotlin
fun main() {
    val name = MyJavaApi.getProperty("name")
    println(name)
}
```

### 범위 함수 사용하기 - apply/with/run/also/let

코틀린은 주어진 객체의 컨텍스트에서 코드 블럭을 실행하기 위한 많은 함수를 제공한다: `let`, `run`, `with`, `apply`, `also`. 알맞는 함수를 고르기 위해서 [범위 함수](링크 추가 예정)를 참조하라.

## 라이브러리 코딩 규약

라이브러리를 작성할 때, API 안정성을 위해 다음의 추가적인 규칙을 따르는 것을 추천한다.
- 항상 멤버 가시성을 명시하라 (실수로 선언을 공식 API로 노출하는 것을 피하기 위해)
- 항상 함수 반환 타입과 프로퍼티 타입을 명시하라 (구현 변경시 실수로 리턴 타입을 변경하는 것을 피하기 위해)
- 새 문서가 필요 없는 오버라이드를 제외한 KDoc 주석을 모든 공식 멤버에게 제공하라 (라이브러리를 위한 문서 작성을 돕기 위해)