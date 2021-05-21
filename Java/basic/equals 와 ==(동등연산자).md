# equals 와 ==(동등 연산자)

`equals` 는 실제 값 또는 주소를 비교하고, `==(동등연산자)`는 주소를 비교한다.

```java
String str1 = "spring"
String str2 = "spring"
String str3 = new String("spring");
String str4 = new String("spring");
```

Java String 의 equals() 메서드 코드를 살펴보면 아래와 같다. 실제 동일한 `주솟값`을 가지는 객체이거나 문자열 `내부의 값(문자들)`이 동일할 경우 true 를 반환한다. 그래서 new 연산자로 생성한 문자열과 리터럴로 생성한 문자열은 내부의 값은 같아서 equals()는 true 를 반환하지만 실제 주소를 비교하는 == 는 false를 반환한 것이다.
동등 비교 연산자인 == 는 실제 객체가 가지고 있는 `주솟값을 비교`하는 연산자이다. 물론 Primitive Type 의 경우 객체가 아니기 때문에 값을 비교하는 것과 마찬가지이다.
