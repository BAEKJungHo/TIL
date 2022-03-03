# Content-Type 에 따라 컨트롤러에서 처리

## MappingJackson2HttpMessageConverter

이 방식은 `@RequestBody` 를 생략할 수 없다. consumes 가 application/json 타입으로 넘어온다.

```
// application/json
{
  "name" : "baek",
  "age" : "29"
}
```

__MappingJackson2HttpMessageConverter 는 내부적으로 `read()` 메서드에서 ObjectMapper 를 사용하여 Object 로 전환해주기 때문에 DTO 에 Setter 가 없어도 된다.__

`objectMapper.readValue(inputMessagge.getBody(), javaType);`

```java
// @RequestBody 생략 불가능 -> @ModelAttribute 가 적용되어 StringHttpMessageConverter 가 적용되어버릴 수 있음
// MappingJackson2HttpMessageConverter (content-type: application/json)
@PostMapping(value = "/add", consumes = MediaType.APPLICATION_JSON_VALUE)
public String postHandlerForJsonRequest(@RequestBody Person person) {
  // 생략
}
```

## StringHttpMessageConverter

앞에 @ModelAttribute 가 붙거나 생략되면 StringHttpMessageConverter 가 동작한다. url 뒤에 key, value 값이 붙어서 전달된다.

```
// urlencoded
name=baek&age=29
```

```java
// StringHttpMessageConverter
@PostMapping(value = "/add", consumes = MediaType.APPLICATION_FORM_URLENCODED_VALUE)
public String postHandlerForFormRequest(Person person) { // @ModelAttribute 생략
  // 생략
}
```

@PathVariable 도 StringHttpMessageConverter 로 동작한다.

```
HTTP/1.1 200 
Request method:	GET
Request URI:	http://localhost:55494/paths?source=1&target=6
Headers: 	Accept=application/json
		Content-Type=application/json; charset=UTF-8
```

```java
// StringHttpMessageConverter
@GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
public ResponseEntity<PathResponse> findShortedPath(PathRequest pathRequest) {
    return ResponseEntity.ok().body(pathService.findShortestPath(pathRequest));
}
```

__StringHttpMessageConverter 가 동작하는 경우 DTO 에 setter 가 존재해야 한다. ObjectMapper 를 사용하지 않기 때문이다.__

setter 를 제거할 수도 있는데

정답은 `initBeanPropertyAccess` 아래에 있다.

바로 `initDirectFieldAccess` 를 사용하는 것이다

해당 메소드를 사용하면 값 할당 방법은 setter가 아닌 Field에 직접 접근한다.

사용하는 방법은 간단하다. 전체 Controller 에서 사용할 수 있게 ControllerAdvice 에 선언하면 된다.

```java
@Slf4j
@ControllerAdvice
public class WebControllerAdvice {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        binder.initDirectFieldAccess();
    }
}
```

## References

- https://blog.naver.com/PostView.naver?blogId=writer0713&logNo=221853596497&redirect=Dlog&widgetTypeCall=true&directAccess=false
