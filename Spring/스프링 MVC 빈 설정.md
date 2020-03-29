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
 ```
