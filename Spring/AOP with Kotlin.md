# AOP With Kotlin

## Spring AOP 를 사용하기 위한 의존성 설정

> @Aspect, @Around 등을 사용하기 위해서 의존성을 설정해야 한다.

```xml
implementation("org.springframework:spring-aop:5.3.18")
implementation("org.springframework:spring-aspects:5.3.18")
```

## References

[코틀린과-Hibernate-CGLIB-Proxy-오해와-재대로된-사용법](https://alkhwa-113.tistory.com/entry/%EC%BD%94%ED%8B%80%EB%A6%B0%EA%B3%BC-Hibernate-CGLIB-Proxy-%EC%9E%AC%EB%8C%80%EB%A1%9C%EB%90%9C-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%EC%98%A4%ED%95%B4-1)

https://stackoverflow.com/questions/64281701/spring-aop-aspectj-with-kotlin-and-gradle-i-cant-get-it-to-work

[Spring Docs. Spring AOP vs AspectJ](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-choosing)
