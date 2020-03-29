# 스프링 MVC 빈 설정

사용자가 특정 URL 로 요청을 보낼때 webapp 까지는 DispatcherServlet 이 구분 해주고 컨트롤러의 @GetMapping("/hello") 처럼 설정된 URL은 
`HandlerMapping` 이 구분을 해준다. 즉 핸들러 매핑은, 어노테이션 기반 매핑으로 핸들러를 찾아준다.

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

## HandlerMapping vs HandlerAdapter

HandlerMapping 은 항상 `BeanNameUrlHandlerMapping` 을 먼저 거치고 온다. (이게 우선순위가 더 높기 때문)

즉, BeanNameUrlHandlerMapping -> RequestMappingHandlerMapping 

하지만 우리는 이제 어노테이션 기반으로 매핑하기 때문에 BeanNameUrlHandlerMapping 를 거치지 않게 된다.
