# @ComponentScan

스프링에서 `<context:component-scan base-packages="경로" />` 이렇게 되어있을때 스프링부트에서는 아래와 같이 선언해 줄 수 있다.

```java
@Configuration
@ComponentScan(basePackages = "kr.a2mvn.largefileupload")
public class NeroConfig {
}
```

단, 이 경우는 자바파일 즉, WEB-INF/lib 안에 jar로 되어있는 클래스 파일인 경우 사용한다. 

만약 Application.java에 `@SpringBootApplication` 어노테이션이 있으면 기본 패키지 경로가 net.xxx.yyy 일때

yyy 폴더 아래에 폴더를 하나 생성해주고, 해당 폴더에 자바 파일을 넣게되면 알아서 컴포넌트스캔을 통하여 @Service, @Controller 등 어노테이션이 있는 애들을
자동으로 빈으로 등록해준다.
