# NEXTSTEP_3주차_STEP2_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-favorite/pull/176)

## STEP2 -인증 로직 리팩터링

### 요구사항

- 1,2단계에서 구현한 인증 로직에 대한 리팩터링을 진행하세요
- 내 정보 수정 / 삭제 기능을 처리하는 기능을 구현하세요.
- Controller에서 @ㅐ너테이션을 활용하여 Login 정보에 접근

### 리팩토링 힌트

- __AuthenticationConverter 추상화__
  - Token Auth와 FormLogin(Session) 으로 나뉘어 있는 AuthenticationConverter를 추상화
  - `Strategy Pattern`

```java
public interface AuthenticationConverter {
    AuthenticationToken convert(HttpServletRequest request);
}
```

- __AuthenticationInterceptor 추상화__
  - AuthenticationInterceptor의 후처리 로직을 추상화
  - `Template Method Pattern`

```java
public abstract void afterAuthentication(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException;
```

- __auth 패키지와 member 패키지에 대한 의존 제거__
  - 현재 auth 패키지와 member 패키지는 서로 의존하고 있음
  - UserDetailsService를 추상화 하여 auth -> member 의존을 제거하기

- __SecurityContextInterceptor 추상화__

```java
public abstract class SecurityContextInterceptor implements HandlerInterceptor {

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        SecurityContextHolder.clearContext();
    }
}
```

> [우아한 객체지향 세미나 - 패키지 의존 문제 해결](https://www.youtube.com/watch?t=2941&v=dJ5C4qRqAgA&feature=youtu.be)

### 느낀점

- 미션을 진행하면서 `PSA(Portable Service Abstraction)` 의 기능이 엄청 중요함을 느꼈다.
- PSA 를 잘 사용하면 `패키지 의존성 제거`를 할 수 있을 뿐만 아니라, `테스트 작성이 쉬운 코드`가 된 다는 것을 느꼈다.
