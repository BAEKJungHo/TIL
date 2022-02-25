# DTO

```kotlin
data class Customer(
  val name: String,
  val email: String
)
```

> `()` 괄호가 생성자라고 생각하면된다. 정확히 말하면 `class body` 라고 한다.

provides a Customer class with the following functionality:

- getters (and setters in case of vars) for all properties
- equals()
- hashCode()
- toString()
- copy()
- component1(), component2(), ..., for all properties (see Data classes)

## Properties declared in the class body

```kotlin
data class Person(val name: String) {
    var age: Int = 0
}
```

data class 는 equals(), hashCode(), toString(), copy() 등을 자동으로 생성해주는데,` class body 내에 있는 속성`을 대상으로 만들어준다.

따라서, 객체를 비교할때 age 값이 달라도, name 만 동일하면 같은 객체라고 판단한다.

```kotlin
val person1 = Person("John")
val person2 = Person("John")
person1.age = 10
person2.age = 20
```

## Copying

copy() 기능을 사용하면 일부 속성에 대해서만 변경하여 카피할 수 있다.

```kotlin
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```

You can then write the following:

```kotlin
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

## Data classes and destructuring declarations

ES6 의 기능중 하나인 `해체 할당(destructuring assignment)`이 kotlin 에도 적용된다.

```kotlin
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age") // prints "Jane, 35 years of age"
```


