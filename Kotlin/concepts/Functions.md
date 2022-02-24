# Functions

- Kotlin functions are declared using the `fun` keyword
- 자바와 다르게 메서드에도 `타입 추론(Type Inferred)` 기능이 있다.

## Type is Inferred

```java
fun sum(a: Int, b: Int): Int {
  return a + b;
}

// Type is Inferred
fun sum(a: Int, b: Int) = a + b
```

## No Meaningful value

- 자바의 Void 타입은 Kotlin 에서 `Unit` 으로 사용된다.

```java
fun printSum(a: Int, b: Int) : Unit {
  println("sum of $a and $b is ${a + b}")
}
```

- Unit 은 생략 가능하다.

```java
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
```

## Parameters

- 매개변수는 `파스칼` 표기법을 따른다.
- 각 매개변수는 콤마로 구분된다.

```java
fun powerOf(number: Int, exponent: Int): Int { /*...*/ }
```

- 자바의 Enum 에만 존재했던 `후행 쉼표(trailing comma)` 가 매개변수에도 적용된다.

```java
fun powerOf(
    number: Int,
    exponent: Int, // trailing comma
) { /*...*/ }
```
