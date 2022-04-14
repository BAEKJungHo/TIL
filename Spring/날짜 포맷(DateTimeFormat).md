# 날짜 포맷

> https://jojoldu.tistory.com/361

```kotlin
// space 를 주게 되면, payload 에 담아서 보내는 과정에서 실수가 발생할 수 있다.
@RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd'T'HH:mm:ss") to: LocalDateTime
```
