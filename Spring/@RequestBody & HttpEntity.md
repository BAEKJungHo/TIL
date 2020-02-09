# @RequestBody & HttpEntity

- @RequestBody
  - 요청 본문에 들어있는 데이터를 HttpMessageConverter를 통해 변환한 객체로 받아올 수 있다.
  - @Valid 또는 @Validate를 통해 검증할 수 있다.
  - BindinResult로 검증 에러를 확인할 수 있다.
  
- HttpMessageConverter
  - 스프링 MVC 설정 WebMvcConfigurer에서 설정할 수 있다.
  - configureMessageConverters : 기본 메시지 컨버터 대체
  - extendMessageConverter : 메시지 컨버터에 추가
  - 기본 컨버터
    - WebMvcConfigurationSupport.addDefaultHttpMessageConverters
    
- HttpEntity
  - @RequestBody와 비슷하지만 추가적으로 요청 헤더 정보를 사용할 수 있다.

```java
@PostMapping
public Event createEvent(HttpEntity<Event> request) {
  // save event
  MediaType mediaType = request.getHeaders().getContentType();
  return reqeust.getBody();
}
```

HttpEntity를 사용할 때는 @RequestBody를 붙이지 않아도 되는데 대신 제네릭 타입에 어떤 자바 객체로 받을 것인지 명시해줘야한다.
