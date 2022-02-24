# Functions

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
