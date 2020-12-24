# 스프링 부트에서 Swagger 설정하기

> [Swagger vs Spring Rest Docs](https://woowabros.github.io/experience/2018/12/28/spring-rest-docs.html)

## 의존성 추가

- swagger 를 사용하기 위한 의존성
- swagger ui(화면)와 관련된 의존성

> https://mvnrepository.com/artifact/io.springfox/springfox-swagger2

```xml
<!-- springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>

<!-- springfox-swagger2 -->
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

> Swagger2(2.9.x) 를 사용하는 경우 swagger-ui.html 에 접근하면 404 Not Found 에러가 발생하는 경우가 있다. 이러한 경우 스프링 부트의 WebConfig 에서 ResourceHandler 에 
 swagger-ui.html 페이지가 어디에 있는지 위치를 추가해야 한다.

## SwaggerConfig 생성

```java
@EnableSwagger2
@Configuration
public class SwaggerConfig {

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Weave")
                .description("API 테스트 및 관리")
                .build();
    }

    @Bean
    public Docket apiDocket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("Mayeye")
                .apiInfo(this.apiInfo())
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.ant("/**/api/**"))
                .build();
    }

}
```

> 특정 패키지 지정하기 : `.apis(RequestHandlerSelectors.basePackage("com.weave.*.*.*.web.rest"))`

- Swagger 설정을 정의한 코드
 - .consume() 과 .produces() 는 각각 Request Content-Type, Response Content-Type 에 대한 설정(선택)
 - .apiInfo() 는 Swagger API 문서에 대한 설명을 표기하는 메소드 (선택)
 - .apis() 는 Swagger API 문서로 만들기 원하는 basePackage 경로 (필수)
 - .path() 는 URL 경로를 지정하여 해당 URL에 해당하는 요청만 Swagger API 문서로 만든다. (필수)
 
 
 ## @Profile
 
 - `@Profile`
  - 운영 프로파일에서는 Swagger 설정이 적용되지 않도록 처리 가능
  
swagger 구성을 별도의 구성 클래스에 넣고 @Profile 을 달아 특정 프로필에서만 Spring 컨텍스트로 스캔되도록한다.

```java
@Configuration
@EnableSwagger2
@Profile({"local", "dev"})
public class SwaggerConfig {
    // your swagger configuration
}
```

- application.yml(.properties) profile 

```
.properties : `spring.profiles.active=dev`
.yml 
    spring:
        profiles:
            active: local
```

## Reference

> https://jojoldu.tistory.com/31
>
> https://yookeun.github.io/java/2017/02/26/java-swagger/
> 
> https://medium.com/@jinnyjinnyjinjin/java-spring-boot-swagger-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0-4f83029bd57b
>
> https://yonguri.tistory.com/87
