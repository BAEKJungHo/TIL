# SpringBoot, Tomcat9 JNDI 설정

Spring Boot 에서 Jar 배포 시 JSP 사용이 불가능하고 webapp 폴더를 무시하기 때문에, jsp 를 사용하는 경우에는 무조건 war 배포를 해야한다. 따라서 pom.xml 패키징 설정이 war 로 되어있어야 하고,
보통 내장 톰캣이 아닌 외장 톰캣을 사용한다.

국내 솔루션 업체들이 license 를 제공하는 방식이 WEB-INF 안에 파일유무를 체크하는데, Jar 로 배포하면 사용하지 못한다.

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

## application.yml 설정

`spring.datasource.jndi-name=java:comp/env/jdbc/TestDB` jndi-name 설정 이름은, tomcat 의 server.xml `<GlobalNamingResources>` 부분의 `<Resource name="jdbc/TestDB" ~~ />` 여기에 설정이 되어 있어야 한다.

JNDI 를 사용하게 되면 DB 커넥션을 WAS 에서 생성하므로, application.yml 에 있는 datasource 설정의 값이 이상하더라도 동작해야한다.

mybatis 를 사용하는 경우 아래와 같이 설정을 해야 한다.

```yml
spring:
  datasource:
    driver-class-name: net.sf.log4jdbc.DriverSpy
    tomcat:
      max-active: 400
      max-wait: 10000
      max-idle: 20
      test-on-borrow: true
      test-while-idle: true
      validation-query: select 1 from dual
      validation-query-timeout: 100000
      time-between-eviction-runs-millis: 600000
    test-on-borrow: true
    test-while-idle: true
    validation-query: select 1 from dual
    validation-query-timeout: 10000
    time-between-eviction-runs-millis: 60000
    jndi-name: java:comp/env/jdbc/TestDB
  http:
    multipart:
      max-file-size: 200MB # 파일 하나 당 업로드 가능 용량
      max-request-size: 200MB # 요청 한번 당 파일 총 업로드 가능 용량
  mvc:
    view:
      prefix: /WEB-INF/jsp/
      suffix: .jsp
  messages:
    basename: messages/message # 국제화 언어 설정

mybatis:
  type-aliases-package: com.weave.core.module
  config-location: classpath:mybatis/config/Mybatis_Config.xml
  mapper-locations: classpath:mybatis/mapper/**/*_MySQL.xml
```

## Application 클래스에서 SpringBootServletInitializer 상속받기

[SpringBootServletInitializer 를 상속 받아야 하는 이유](https://github.com/BAEKJungHo/TIL/blob/master/Spring/SpringBootServletInitializer%20%EB%A5%BC%20%EC%83%81%EC%86%8D%20%EB%B0%9B%EC%95%84%EC%95%BC%20%ED%95%98%EB%8A%94%20%EC%9D%B4%EC%9C%A0.md)

> Tomcat 8 이상, JDK 7 이상은 Servlet 3.1 이상의 버전을 사용한다.

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

## war 파일 추출

SNAPSHOT.war

## tomcat 의 webapps 안에 war 파일 넣기

- ex) C:\Users\weave\Desktop\dev\apache-tomcat-9.0.45\webapps
- war 파일명 수정 (ex. SNAPSHOT.war)

## tomcat server.xml 설정

> tomcat 폴더 - conf - server.xml

`<GlobalNamingResources>` 부분 설정

```xml
  <!-- Global JNDI resources
       Documentation at /docs/jndi-resources-howto.html
  -->
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />

     <Resource name="jdbc/TestDB"
	global="jdbc/TestDB"
	auth="Container"
	type="javax.sql.DataSource"
	driverClassName="net.sf.log4jdbc.DriverSpy"
	url="jdbc:log4jdbc:mysql://192.168.0.1:3307/testDB"
	username="test"
	password="test"
	maxTotal="100"
	maxIdle="20"
	minIdle="5"
	maxWaitMillis="10000" />
  </GlobalNamingResources>
```

## tomcat context.xml 설정

> tomcat 폴더 - conf - context.xml

`<ResourceLink>` 부분 설정 name 과 global 에 server.xml 에서 설정한 것처럼 jdbc/TestDB 이름으로 들어가야한다.

```xml
<Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- 이 부분을 추가해야 함                                  -->
    <ResourceLink name="jdbc/TestDB" global="jdbc/TestDB" auth="Container" type="javax.sql.DataSource" />

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
</Context>
```

## JAVA_HOME 환경 변수 설정

- 시스템 변수에 JAVA_HOME 추가

> https://deftkang.tistory.com/99

## 외부 톰캣 실행

- cmd > tomcat dir > bin > catalina.bat
- http://localhost:8080/SNAPSHOT/cms

## References

> https://www.youtube.com/watch?v=Kg0ZSHKT3Qw
> 
> [JNDI 의 장점](https://ss-o.tistory.com/133)
