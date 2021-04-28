# SpringBoot, Tomcat9 JNDI 설정

## pom.xml 설정

내장 톰캣이 아닌 외부 톰캣을 사용할 것이기 때문에 의존성 설정을 해야 한다.

- 패키징 war 설정 : `<packaging>war</packaging>`
- spring-boot-starter-tomcat scope 설정 : `<scope>provided</scope>`
  - [provided 에 대한 설명](https://www.baeldung.com/maven-dependency-scopes#2-provided)

> This scope is used to mark dependencies that should be provided at runtime by JDK or a container, hence the name.
>
> A good use case for this scope would be a __web application deployed in some container__, where the container already provides some libraries itself.
>
> For example, a web server that already provides the Servlet API at runtime, thus in our project, those dependencies can be defined with the provided scope:
>
> provided 설정은 [Servlet API](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/) 와 [JavaEE API](https://docs.oracle.com/javaee/7/api/toc.htm) 를 runtime 에 웹 컨테이너가 제공한다.

## tomcat server.xml 설정

> tomcat 폴더 - conf - server.xml

`<GlobalNamingResources>` 부분 설정

## application.yml 설정

`spring.datasource.jndi-name=java:comp/env/jdbc/TestDB` jndi-name 설정 이름은, tomcat 의 server.xml `<GlobalNamingResources>` 부분의 `<Resource name="jdbc/TestDB" ~~ />` 여기에 설정이 되어 있어야 한다.

## tomcat context.xml 설정

> tomcat 폴더 - conf - context.xml

`<ResourceLink>` 부분 설정 name 과 global 에 server.xml 에서 설정한 것처럼 jdbc/TestDB 이름으로 들어가야한다.

## Application 클래스에서 SpringBootServletInitializer 상속받기

[SpringBootServletInitializer 를 상속 받아야 하는 이유](https://github.com/BAEKJungHo/TIL/blob/master/Spring/SpringBootServletInitializer%20%EB%A5%BC%20%EC%83%81%EC%86%8D%20%EB%B0%9B%EC%95%84%EC%95%BC%20%ED%95%98%EB%8A%94%20%EC%9D%B4%EC%9C%A0.md)

```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }
  
  /**
   * Tomcat 서버가 시작될 때 호출된다.
   * WAS 에 war 파일을 배포할 때 Spring Boot 의 내용을 Servlet 으로 초기화
   * war 프로젝트로 생성할 경우 위와 같이 WAS 에 deploy 될 때 Spring Boot Application 을 Servlet 으로 등록하여 서비스할 수 있게 해준다.
   */
  @Override
  protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
    return builder.sources(Application.class);
  }

}
```

## References

> https://www.youtube.com/watch?v=Kg0ZSHKT3Qw
