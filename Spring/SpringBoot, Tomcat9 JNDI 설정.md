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

## References

> https://www.youtube.com/watch?v=Kg0ZSHKT3Qw
