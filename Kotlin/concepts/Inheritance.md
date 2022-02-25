# Inheritance

Kotlin 의 모든 클래스는 기본적으로 `final` 로 선언된다. 따라서, 기본적으로 상속이 불가능하다.
상속을 가능하게 하려면 `open` 이라는 키워드를 class 앞에 붙이면 된다.

```kotlin
open class Base // Class is open for inheritance
```

```kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```

```kotlin
class MyView : View {
    constructor(ctx: Context) : super(ctx)

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

## Overriding Methods

```kotlin
open class Shape {
    open fun draw() { /*...*/ }
    fun fill() { /*...*/ }
}

class Circle() : Shape() {
    override fun draw() { /*...*/ }
}
```

- `override` 할때에는 해당 키워드를 꼭 적어줘야 한다. 안그러면 컴파일 시점에 에러가 발생한다.
- Shpae.fill() 의 경우 open 키워드가 없는데, 이러한 경우에는 하위 클래스에서 동일한 시그니처를 갖는 메서드를 만들 수 없다.
- 하위 클래스에서 override 된 메서드를 재정의 할 수 없게 하려면 `final`을 붙이면 된다.

```kotlin
open class Rectangle() : Shape() {
    final override fun draw() { /*...*/ }
}
```

## Overriding Properties

- 속성도 오버라이딩 할 수 있다.
  - val -> val (O)
  - val -> var(O)
    - 이게 가능한 이유는, val 은 기본적으로 get 을 가지고 있고 var 로 오버라이딩하는 순간 set 을 추가하기 때문이다.
  - var -> val (X)

```kotlin
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape // Always has 4 vertices

class Polygon : Shape {
    override var vertexCount: Int = 0  // Can be set to any number later
}
```

