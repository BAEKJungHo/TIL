## Profiles 가 나눠져있을때 테스트 코드를 실행하기 위한 설정하기

```yml
spring:
  profiles:
    dev: database-dev, ...
```

이런식으로 되어 있을 때, 테스트 코드에서 `@SpringBootTest` 을 사용하여 DI 를 제대로 하기 위해서는 실행하기 전 Configuration 에서 `VM options` 를 줘야 한다.

`-Dspring.profiles.active=development`

기존에 다른게 설정이 되어있다면 띄어쓰기 후에 진행

```
-ea -Dspring.profiles.active=development
```
