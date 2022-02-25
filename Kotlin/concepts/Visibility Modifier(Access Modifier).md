# Visibility Modifier

Kotlin 에서는 `가시성 수정자(Visibility Modifier)`라고 부르며, 자바에서는 `접근 제한자(Access Modifier)`라고 부른다.

- 가시성 수정자를 사용하지 않으면 public 기본적으로 가 사용된다. 즉, 선언이 모든 곳에서 표시된다.
- 선언을 로 표시하면 선언 private 이 포함된 파일 내에서만 볼 수 있다.
- internal 로 표시 하면 동일한 모듈의 모든 곳에서 볼 수 있다 .
- protected 수정자는 최상위 선언에 사용할 수 없다.

```kotlin
// file name: example.kt
package foo

private fun foo() { ... } // visible inside example.kt

public var bar: Int = 5 // property is visible everywhere
    private set         // setter is visible only in example.kt

internal val baz = 6    // visible inside the same module
```

## Class Member

클래스 내부에 선언된 멤버의 경우에는 다음과 같다.

- private 멤버가 이 클래스 내에서만 볼 수 있음을 의미한다.(모든 멤버 포함)
- protected 멤버는 private 으로 표시된 멤버와 가시성이 동일 하지만 하위 클래스에서도 볼 수 있음을 의미한다.
- internal 선언하는 클래스를 보는 이 모듈 내부 의 모든 클라이언트 는 해당 internal 멤버를 볼 수 있음을 의미한다.
- public 선언하는 클래스를 보는 모든 클라이언트는 해당 public 멤버를 볼 수 있음을 의미한다.

```kotlin
open class Outer {
    private val a = 1
    protected open val b = 2
    internal open val c = 3
    val d = 4  // public by default

    protected class Nested {
        public val e: Int = 5
    }
}

class Subclass : Outer() {
    // a is not visible
    // b, c and d are visible
    // Nested and e are visible

    override val b = 5   // 'b' is protected
    override val c = 7   // 'c' is internal
}

class Unrelated(o: Outer) {
    // o.a, o.b are not visible
    // o.c and o.d are visible (same module)
    // Outer.Nested is not visible, and Nested::e is not visible either
}
```
