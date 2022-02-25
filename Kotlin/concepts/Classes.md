# Classes

Classes in Kotlin are declared using the keyword `class`

```kotlin
class Person { /*...*/ }
```

- __클래스 구조__
  - `Name`
    - 클래스 이름 
  - `Header`
    - 클래스 헤더 : 매개변수와 타입, 기본 생성자 등
  - `Body`
    - 클래스 본문
    - 클래스에 본문이 없으면 중괄호를 생략할 수 있다.

```kotlin
class Empty
```

## 생성자

- Kotlin 의 클래스에는 기본 생성자 와 하나 이상의 보조 생성자 가 있을 수 있다.
- 기본 생성자는 클래스 헤더의 일부이며 클래스 이름과 선택적 유형 매개변수 뒤에 온다.
- 기본적으로 모든 생성자는 `public` 이다. 
   - internal 클래스의 생성자는 동일한 모듈 내에서만 볼 수 있다.

```kotlin
class Person constructor(firstName: String) { /*...*/ }
```

기본 생성자에 주석이나 가시성 수정자가 없으면 constructor 키워드를 생략할 수 있다.

```kotlin
class Person(firstName: String) { /*...*/ }
```

## 인스턴스 초기화 블록

인스턴스가 생성될때, 초기화 블록이 있는지 확인하고 초기화를 수행한다.

```kotlin
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints $name")
    }
    
    val secondProperty = "Second property: ${name.length}".also(::println)
    
    init {
        println("Second initializer block that prints ${name.length}")
    }
}
```

초기화 블록에서 기본 생성자 매개변수를 사용할 수 있다. 또한 class body 에 있는 속성도 사용할 수 있다.

- __속성을 선언하고 기본 생성자에서 초기화__

```kotlin
class Person(val firstName: String, val lastName: String, var age: Int)
```

클래스 속성의 기본값도 포함될 수 있다.

```kotlin
class Person(val firstName: String, val lastName: String, var isEmployed: Boolean = true)
```
