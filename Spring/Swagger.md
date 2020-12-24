# 스프링 부트에서 Swagger 설정하기

## 의존성 추가

- swagger 를 사용하기 위한 의존성
- swagger ui(화면)와 관련된 의존성

> https://mvnrepository.com/artifact/io.springfox/springfox-swagger2

```xml
<!-- springfox-swagger2 -->
<dependency>
 <groupId>io.springfox</groupId>
 <artifactId>springfox-swagger-ui</artifactId>
 <version>2.9.2</version>
</dependency>
<!-- springfox-swager-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
```

## resourceHandler 설정

css, image 파일 같은 정적 파일 자원을 스프링에서는 `<mvc:resources>` 를 이용해서 매핑 해줘야 View 단에서 제대로 정적 파일을 찾을 수 있는데, 스프링 부트는 `spring-boot-starter-web` 에서
알아서 처리해준다.

만약에 jar 안에 있는 경로나, 클래스 경로 등에 대해서 정적 파일을 사용하기 위해서는 등록을 해줘야 하는데 `WebMvcConfigurerAdapter` 를 상속받은 WebConfig 에서 `addResourceHandlers` 를 오버라이딩하여
구현하면 된다.

> 웹 브라우저에서 효율적인 로딩을 위해 최적화 된 캐시 헤더 설정을 포함하여 Spring MVC를 통해 이미지, CSS 파일 등의 정적 리소스를 제공하기위한 리소스 핸들러의 등록을 저장합니다. 리소스는 웹 애플리케이션 루트 아래의 위치, 클래스 경로 및 기타 위치에서 제공 될 수 있습니다.
>
> https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ResourceHandlerRegistry.html

```java
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
```

## SwaggerConfig 생성

```java


## Reference

> https://yookeun.github.io/java/2017/02/26/java-swagger/
> 
> https://medium.com/@jinnyjinnyjinjin/java-spring-boot-swagger-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0-4f83029bd57b
