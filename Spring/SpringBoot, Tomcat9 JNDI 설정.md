# SpringBoot, Tomcat9 JNDI 설정

## pom.xml 설정

내장 톰캣이 아닌 외부 톰캣을 사용할 것이기 때문에 의존성 설정을 해야 한다.

- 패키징 war 설정 : `<packaging>war</packaging>`
- spring-boot-starter-tomcat scope 설정 : `<scope>provided</scope>`
