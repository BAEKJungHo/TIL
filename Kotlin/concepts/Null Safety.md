# Null Safety

Kotlin 은 [The Billion Dollar Mistake](https://en.wikipedia.org/wiki/Tony_Hoare#Apologies_and_retractions) 라고 하는 null 참조의 위험을 제거하는것을 목표로 설계되었다.

> Java 를 포함한 많은 프로그래밍 언어에서 가장 일반적인 함정 중 하나는 null 참조의 멤버에 액세스하면 null 참조 예외가 발생한다는 것이다.
> Java 에서 이것은 NullPointerException, 또는 줄여서 NPE 와 동일하다.

## Kotiln 에서 NPE 가 발생할 수 있는 유일한 원인

- `throw NullPointerException()` 호출
- `!!` 연산자 사용
- 생성자에서 초기화되지 않은 this 가 어딘가에서 사용되는 경우(`leaking this`)
- 슈퍼클래스 생성자는 파생 클래스 의 구현이 초기화되지 않은 상태를 사용하는 멤버를 호출하는 경우.

## Non-null

```kotlin
var a: String = "abc" // Regular initialization means non-null by default
a = null // compilation error
```

## nullable

```kotlin
var b: String? = "abc" // can be set to null
b = null // ok
print(b)
```

## 조건에서 null 확인

먼저 명시적으로 b 의 null 여부를 확인 하고 두 옵션을 별도로 처리할 수 있다.

```kotlin
val l = if (b != null) b.length else -1
```

```kotlin
// b 가 immutable 해야만 가능한 코드
val b: String? = "Kotlin"
if (b != null && b.length > 0) {
    print("String of length ${b.length}")
} else {
    print("Empty string")
}
```

## Safe calls

`?.` 연산자를 사용하여 nullable 한 변수에 안전하게 접근할 수 있다.

```kotlin
val a = "Kotlin"
val b: String? = null
println(b?.length)
println(a?.length) // Unnecessary safe call
```

안전한 호출은 체인에서 유용하다. 
예를 들어, Bob은 부서에 배정되거나 배정되지 않을 수 있는 직원이다. 
해당 부서에는 다른 직원이 부서장으로 있을 수 있다.
Bob의 부서장(있는 경우)의 이름을 얻으려면 다음을 작성한다.

```kotlin
bob?.department?.head?.name
```

```kotlin
// If either `person` or `person.department` is null, the function is not called:
person?.department?.head = managersPool.getManager()
```

### let

null 이 아닌 값에 대해서만 특정 작업을 수행하려면 `let`을 사용하면된다.

```kotlin
val listWithNulls: List<String?> = listOf("Kotlin", null)
for (item in listWithNulls) {
    item?.let { println(it) } // prints Kotlin and ignores null
}
```

## Elvis Operator

- 왼쪽에 있는 표현식 `?:` 이 not null 이면 Elvis 연산자가 이를 반환하고, 그렇지 않으면(null 이면) 오른쪽 반환

```kotlin
val l = b?.length ?: -1
```
```kotlin
fun foo(node: Node): String? {
    val parent = node.getParent() ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("name expected")
    // ...
}
```

## The !! operator

`!!` 연산자는 `the not-null assertion operator` 이다. 

```kotlin
// b 가 null 이면 NPE 발생, null 이 아니면 b.length 반환
val l = b!!.length
```

## Safe cast

```kotlin
// a 가 Int 가 아니면 ClassCastException 가 아닌 nul 을 반환한다.
val aInt: Int? = a as? Int
```

## Collections of a nullable type

If you have a collection of elements of a nullable type and want to filter non-null elements, you can do so by using filterNotNull

```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()
```

