# @Value

- 불변 클래스를 만들 때 사용.
- 필드에 private final 을 붙여서 생성해준다.
- 클래스도 final 을 붙여서 생성해준다.
- equals, hashCode, toString 을 만들어준다.
- 기본 생성자는 private 생성자이다.

- 롬복 사용

```java
@Value
public class ValueExample {
    String name;
    String email;
}
```

- 바닐라 자바

```java
public final class ValueExample {
    private final String name;
    private final String email;

    public ValueExample(String name, String email) {
        this.name = name;
        this.email = email;
    }

    private ValueExample() {
        this.name = null;
        this.email = null;
    }

    public String getName() {
        return this.name;
    }

    public String getEmail() {
        return this.email;
    }
    // equals, hashCode, toString
}
```
