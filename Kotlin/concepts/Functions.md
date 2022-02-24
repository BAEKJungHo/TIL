# Functions

- Kotlin functions are declared using the `fun` keyword
- 자바와 다르게 메서드에도 `타입 추론(Type Inferred)` 기능이 있다.

## Type is Inferred

```kotlin
fun sum(a: Int, b: Int): Int {
  return a + b;
}

// Type is Inferred
fun sum(a: Int, b: Int) = a + b
```

## No Meaningful value

- 자바의 Void 타입은 Kotlin 에서 `Unit` 으로 사용된다.

```kotlin
fun printSum(a: Int, b: Int) : Unit {
  println("sum of $a and $b is ${a + b}")
}
```

- Unit 은 생략 가능하다.

```kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
```

## Parameters

- 매개변수는 `파스칼` 표기법을 따른다.
- 각 매개변수는 콤마로 구분된다.

```kotlin
fun powerOf(number: Int, exponent: Int): Int { /*...*/ }
```

- 자바의 Enum 에만 존재했던 `후행 쉼표(trailing comma)` 가 매개변수에도 적용된다.

```kotlin
fun powerOf(
    number: Int,
    exponent: Int, // trailing comma
) { /*...*/ }
```

## Default arguments

- 매개변수는 `기본값`을 가질 수 있다. 해당 인수를 건너뛸 때 사용되며, 이렇게 하면 과부하 수가 줄어든다.

```kotlin
fun read(
    b: ByteArray,
    off: Int = 0,
    len: Int = b.size,
) { /*...*/ }
```

## Function Overriding

- 재정의 메서드는 항상 기본 메서드와 동일한 기본 매개 변수 타입을 사용한다.
- 기본 매개변수 값이 있는 메서드를 재정의할 때 기본 매개변수 값은 서명에서 생략되어야 한다.

```java
open class A {
    open fun foo(i: Int = 10) { /*...*/ }
}

class B : A() {
    override fun foo(i: Int) { /*...*/ }  // No default value is allowed.
}
```


