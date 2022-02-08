# HttpServletRequest 의 Body 에 담긴 JSON 을 객체로 변환

- __테스트 코드__

```java
public static MockHttpServletRequest createMockRequest() throws IOException {
    MockHttpServletRequest request = new MockHttpServletRequest();
    TokenRequest tokenRequest = new TokenRequest(EMAIL, PASSWORD);
    
    // body 에 JSON 문자열 형태로 담기
    request.setContent(new ObjectMapper().writeValueAsString(tokenRequest).getBytes());
    return request;
}
```

- __변환__

```java
public AuthenticationToken convert(HttpServletRequest request) throws IOException {
    TokenRequest tokenRequest = new ObjectMapper().readValue(request.getInputStream(), TokenRequest.class);
    // 생략
}
```
