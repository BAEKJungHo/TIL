# val, var

- __val__
  - value 라는 의미
  - immutable 한 변수를 저장한다.
  - 자바의 final 이 적용되었다고 보면 된다.
    - 즉, 변수의 경우 값 or 객체의 재할당이 불가능하다. (객체 내부에 있는 값을 변경할 수는 있음)
- __var__
  - variable 라는 의미
    - 변수라는 의미도 있지만, 형용사로 쓰이면 `변할 수 있는`이라는 의미를 가지고 있다.
  - mutable 한 변수를 저장한다. 따라서, 값이 바뀔 수 있다.

올바른 사용법은 무조건 `val` 을 습관화 하고, 필요할 때 `var` 를 사용하는 것이 좋다.

## Property

- 자바에서는 필드 혹은 변수라고 부르는데, 코틀린에서는 클래스 변수를 `프로퍼티(property)`라고 부른다.
  - Property : 필드 + 접근자 메서드
- `var` 또는 `val` 키워드를 사용하여 만든다.

> 접근자라고 부르는 getter, equals, hashCode 와 같은 함수가 내장되어 있기 때문에 프로퍼티라고 불린다.

## Getter, Setter

```kotlin
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```

### 사용자 지정 getter 

```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
```

타입 추론이 가능한 경우, 프로퍼티 타입을 생략할 수 있다.

```kotlin
val area get() = this.width * this.height
```

### 사용자 지정 setter

```kotlin
var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value) // parses the string and assigns values to other properties
    }
```

#### setter 에 접근 지정자 설정

```kotlin
var setterVisibility: String = "abc"
    private set // the setter is private and has the default implementation

var setterWithAnnotation: Any? = null
    @Inject set // annotate the setter with Inject
```

## 컴파일 타임 상수

`const` 수정자를 사용하여 `컴파일 타임 상수`로 표현할 수 있다.

- __조건__
  - It must be a top-level property, or a member of an `object` declaration or a `companion object`.
  - It must be initialized with a value of type `String` or a `primitive type`
  - It cannot be a custom getter

The compiler will inline usages of the constant, replacing the reference to the constant with its actual value. However, the field will not be removed and therefore can be interacted with using reflection.

Such properties can also be used in annotations:

```kotlin
const val SUBSYSTEM_DEPRECATED: String = "This subsystem is deprecated"

@Deprecated(SUBSYSTEM_DEPRECATED) fun foo() { ... }
```

## Backing Fields

`뒷 받침하는 필드(Backing Fields)` 는 말 그대로 프로퍼티의 필드를 나타낸다.

> 프로퍼티는 필드와 접근 메서드를 통틀어 칭하는 단어이다.

```kotlin
class Person(name: String, age: Int) { 
val name = name 
  get() { 
    return field
  } 
var age = age 
  get() { 
    return field 
  } 
  set(value) { 
    field = value 
  } 
}
```


## @field

- https://stackoverflow.com/questions/59925099/what-is-the-purpose-of-using-fieldserializedname-annotation-instead-of-serial
