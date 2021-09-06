# @ComponentScan

스프링에서 `<context:component-scan base-packages="경로" />` 이렇게 되어있을때 스프링부트에서는 아래와 같이 선언해 줄 수 있다.

```java
@Configuration
@ComponentScan(basePackages = "kr.a2mvn.largefileupload")
public class NeroConfig {
}
```

단, 이 경우는 자바파일 즉, WEB-INF/lib 안에 jar 로 되어있는 클래스 파일인 경우 사용한다. 

만약 Application.java 에 `@SpringBootApplication` 어노테이션이 있으면 기본 패키지 경로가 net.xxx.yyy 일때

yyy 폴더 아래에 폴더를 하나 생성해주고, 해당 폴더에 자바 파일을 넣게되면 알아서 컴포넌트스캔을 통하여 @Service, @Controller 등 어노테이션이 있는 애들을 자동으로 빈으로 등록해준다.

## 동작 방식

Configuration 클래스 및 Annotation 에 사용하는 설정들을 파싱한다. 그리고 basePackage 밑의 모든 .class 자원을 불러와서 component 후보인지 확인하여 BeanDefinition(빈 생성을 위한 정의)을 만든다. 생성된 빈 정의를 바탕으로 빈을 생성하고 의존성있는 빈들을 주입한다.
