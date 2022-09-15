# bootJar, jar

멀티모듈을 구성할때 유용

```gradle
jar.enabled = false
bootJar.enabled = true
```

bootJar 을 사용하면 실행가능한 jar(executable jar)를 만들 수 있다.

두 옵션이 모두 true 라면, bootJar 가 실행된 후 jar 옵션이 실행되는데 같은 경로에 jar 파일 생성하게 되어 executable jar 를 덮게된다.

따라서 jar 옵션은 비활성화, bootJar 옵션을 활성화 해야 한다.

## Links

- https://blog.leocat.kr/notes/2020/01/23/gradle-create-executable-jar-with-spring-boot
