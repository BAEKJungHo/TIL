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

## 보조 생성자

```kotlin
class Person(val pets: MutableList<Pet> = mutableListOf())

class Pet {
    constructor(owner: Person) {
        owner.pets.add(this) // adds this pet to the list of its owner's pets
    }
}
```

> 나중에 객체 연관관계 설정 할때, 이러한 코드 많이 사용될 듯 함

클래스에 기본 생성자가 있는 경우 각 보조 생성자는 다른 보조 생성자를 통해 직접 또는 간접적으로 기본 생성자에 `위임`해야 한다. 동일한 클래스의 다른 생성자에 대한 위임은 다음 `this` 키워드를 사용하여 수행된다.

```kotlin
class Person(val name: String) {
    val children: MutableList<Person> = mutableListOf()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```

## 클래스의 인스턴스 생성

클래스의 인스턴스를 생성하려면 생성자가 일반 함수인 것처럼 호출하면 된다.

> Kotlin 에는 new 키워드가 없다.

```kotlin
val invoice = Invoice()
val customer = Customer("Joe Smith")
```

## 모든 클래스의 슈퍼 클래스 : Any

Kotlin 에서 모든 클래스는 `Any` 라는 슈퍼 클래스를 갖는다.

```kotlin
class Example // Implicitly inherits from Any
```

Any has three methods: `equals(), hashCode(), and toString()`. Thus, these methods are defined for all Kotlin classes.
