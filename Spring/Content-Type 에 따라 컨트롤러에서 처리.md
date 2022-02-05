# Content-Type 에 따라 컨트롤러에서 처리

```java
// @RequestBody 생략 불가능 -> @ModelAttribute 가 적용되어 StringHttpMessageConverter 가 적용되어버릴 수 있음
// MappingJackson2HttpMessageConverter (content-type: application/json)
@PostMapping(value = "/add", consumes = MediaType.APPLICATION_JSON_VALUE)
public String postHandlerForJsonRequest(@RequestBody Person person) {
  // 생략
}

// StringHttpMessageConverter (content-type: application/x-www-form-urlencoded)
@PostMapping(value = "/add", consumes = MediaType.APPLICATION_FORM_URLENCODED_VALUE)
public String postHandlerForFormRequest(Person person) { // @ModelAttribute 생략
  // 생략
}
```

```
// urlencoded
name=baek&age=29

// application/json
{
  "name" : "baek",
  "age" : "29"
}
```

## References

- https://blog.naver.com/PostView.naver?blogId=writer0713&logNo=221853596497&redirect=Dlog&widgetTypeCall=true&directAccess=false
