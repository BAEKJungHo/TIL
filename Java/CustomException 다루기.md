# 커스텀 익셉션 다루기

```java
@Getter
public class XXXException extends RuntimeException {
    public XXXException(String message) {
        super(message);
    }
}
```

이런식으로 원하는 상위 Exception 을 상속받아서 사용할 수 있다.
