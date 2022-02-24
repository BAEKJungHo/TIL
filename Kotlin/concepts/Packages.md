# Packages

- 자바와 마찬가지로 import 로 정적인 의존 관계를 나타낼 수 있다.
- 다른 점은 패키지 이름 충돌이 있는 경우 `as` 키워드를 사용하여 Conflict entity 이름을 변경할 수 있다.
  - ```java
    import org.example.Message
    import org.test.Message as testMessage
    ```
