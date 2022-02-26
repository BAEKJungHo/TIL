# Late-initialized properties and variables

일반적으로 null 이 아닌 값을 갖는 프로퍼티는 생성자에서 초기화되어야 한다. 하지만 의존성 주입을 통해 초기화 하거나 단위 테스트를 해야하는 경우,
생성자에서 null 이 아닌 초기화를 제공할 수 없지만, null 검사를 피하고 싶을 때, `lateinit` 수정자를 사용하여 해결할 수 있다.

```kotlin
public class MyTest {
  lateinit var subject: TestSubject
  
  @SetUp fun setUp() {
      subject = TestSubject() 
  }
  
  @Test fun test() {
      subject.method() 
  }
}
```

`lateinit`은 class body 내에 `var` 로 선언된 프로퍼티와, 최상위 프로퍼티 및 지역 변수에 사용할 수 있다. 또한, 프로퍼티 또는 변수의 유형은 non-null 이어야 하며 primitive type 이 아니어야 한다.

## lateinit var 초기화 여부 확인

`lateinit var` 가 초기화 되었는지 확인하려면 `.isInitialized` 를 사용하면 된다.

```kotlin
if (foo::bar.isInitialized) {
  println(foo.bar) 
}
```
