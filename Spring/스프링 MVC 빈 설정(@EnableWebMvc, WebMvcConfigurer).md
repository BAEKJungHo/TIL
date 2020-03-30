# 스프링 MVC 빈 설정

`HandlerInterceptor`는 모든 핸들러 매핑에 설정할 수 있으며, 빈으로 등록할 수 있다.

```java
@Configuration
@ComponentScan
public class WebConfig {
  
  @Bean
  public HandlerMapping handlerMapping() {
    ReqeustMappingHandlerMapping halderMapping = new RequestHanlderMappong();
    halderMapping.setInterceptors();
    hadlerMapping.setOrder(Ordered.HIGHEST_PRECEDENCE);
    return handlerMapping;
  }
  
  @Bean
  public HandlerAdapter handlerAdapter() {
    // handlerAdapter 에 MessageConverter 를 설정 할 수있다.
    // MessageConverter 는 요청본문, 응답본문 처리할 때 사용한다. (@RequestBody, @ResponseBody)
    ReqeustMappingHandlerAdapter handlerAdapter = new ReqeustMappingHandlerAdapter();
  }
}
 ```

위와 같은 설정 방법은 low level 설정 방법이다. 보통은 스프링 부트가 대부분 알아서 설정해준다.

## @EnableWebMvc

@EnableWebMvc 는 MVC 기반으로 기본 설정을 지원 해주는 어노테이션이다.

@EnableWebMvc 내부를 까보면 DelegatingWebMvcConfiguration 이 있고 이 클래스는 `WebMvcConfigurationSupport` 클래스를 
상속 받고 있다. WebMvcConfigurationSupport 에 가보면 기본적으로 빈 들이 등록 되어 있다.

```java
@Configuration
@ComponentScan
@EnableWebMvc
public class WebConfig {
  
}
```

@EnableWebMvc 는 `Delegation (위임)` 구조로 되어있는데 그 이유는 확장성 때문이다. 따라서 `WebMvcConfigurer 인터페이스` 를 상속 받을 수 있다.

### WebMvConfigurer

```java
@Configuration
@ComponentScan
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
  
  @Override
  public void configureViewREsolvers(ViewResolverRegistry registry) {
    registry.jsp("/WEB-INF", ".jsp"); // prefix, suffix
  }
  
}
```

이렇게 WebConfig 클래스에서 ViewResolver를 설정해주면 컨트롤러 핸들러 메서드에서 @GetMapping("/sample") 처럼 사용해도

/WEB-INF/sample.jsp 를 찾게 된다.

## HandlerMapping vs HandlerAdapter

HandlerMapping 은 항상 `BeanNameUrlHandlerMapping` 을 먼저 거치고 온다. (이게 우선순위가 더 높기 때문)

즉, BeanNameUrlHandlerMapping -> RequestMappingHandlerMapping 

하지만 우리는 이제 어노테이션 기반으로 매핑하기 때문에 BeanNameUrlHandlerMapping 를 거치지 않게 된다.

## 그 밖의 WebMvcConfigurer

- 아규먼트 리졸버 설정
  - 스프링 MVC 가 제공하는 기본 아규먼트 리졸버 이외에 커스텀한 아규먼트 리졸버를 추가하고 싶을 때 설정
- CORS 설정
  - Cross Origin 요청 처리 설정
  - 같은 도메인에서 온 요청이 아니더라도 처리를 허용하고 싶다면 설정
- 리턴 값 핸들러 설정
  - 스프링 MVC가 제공하는 기본 리턴값 핸들러 이외에 리턴 핸들러를 추가하고 싶을 때 설정
- 뷰 컨트롤러
  - 단순하게 요청 URL 을 특정 뷰로 연결하고 싶을 때 사용
- 비동기 설정
  - 비동기 요청 처리에 사용할 타임아웃이나 TaskExecutor 설정
- 뷰 리졸버 설정
  - 핸들러에서 리턴하는 뷰 이름에 해당하는 문자열을 View 인스턴스로 바꿔줄 뷰 리졸버 설정
- Content Negotiation 설정
  - 요청 본문 또는 응답 본문을 어떤 MIME 타입으로 보내야 하는지 결정하는 전략을 설정
