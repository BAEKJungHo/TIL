# @Spy

@Spy 를 사용하면 특정 메서드는 STUB 하고, 특정 메서드는 그대로 기존 기능을 사용하도록 할 수 있다.

```java
@ExtendWith(MockitoExtension.class)
class TokenAuthenticationInterceptorTest {
    @Mock
    private CustomUserDetailsService customUserDetailsService;
    @Mock
    private JwtTokenProvider jwtTokenProvider;
    @Spy
    private ObjectMapper objectMapper;
    
    // 생략
    
}
```

이렇게 @Spy 를 활용하면 ObjectMapper 의 기존 기능을 Test Double 을 만들지 않고 그냥 사용할 수 있다.

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws IOException {
    AuthenticationToken authenticationToken = convert(request);
    Authentication authentication = authenticate(authenticationToken);

    String payload = objectMapper.writeValueAsString(authentication.getPrincipal());
    String jwtToken = jwtTokenProvider.createToken(payload);

    TokenResponse tokenResponse = TokenResponse.of(jwtToken);

    String responseToClient = objectMapper.writeValueAsString(tokenResponse);
    response.setStatus(HttpServletResponse.SC_OK);
    response.setContentType(MediaType.APPLICATION_JSON_VALUE);
    response.getOutputStream().print(responseToClient);

    return false;
}
```
