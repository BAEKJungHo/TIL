# Content-Type 에 따라 컨트롤러에서 처리

```java
@PostMapping(value = "/add", consumes = MediaType.APPLICATION_JSON_VALUE)
public String postHandlerForJsonRequest(@RequestBody Person person) {
  // 생략
}

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

## References

- https://blog.naver.com/PostView.naver?blogId=writer0713&logNo=221853596497&redirect=Dlog&widgetTypeCall=true&directAccess=false
