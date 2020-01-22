## X-Frame-Options to DENY 설정 때문에 iframe을 통해 접근할 수 없는 문제 해결

현재 slipp.net은 facebook.com의 앱으로 등록되어 있어 facebook 내에서도 접근이 가능하다. facebook 앱으로 접근하고 싶다면 SLiPP 페이스북 앱로 접근할 수 있다.

그런데 어느 시점부터 페이스북 앱을 통한 접근이 되지 않는 것을 확인했다. 이유를 파악해 보니 facebook이 iframe을 통해 접근하고 있는데 http 응답 헤더의 X-Frame-Options 설정이 DENY로 설정되어 있어 접근할 수 없기 때문이었다. 크롬 개발자 도구를 통해 확인한 에러 메시지는 다음과 같다.

`Refused to display 'https://slipp.net/' in a frame because it set 'X-Frame-Options' to 'DENY'.`

최근에는 iframe을 사용하지 않는 경우가 많은데 동일 도메인에서 iframe을 통해 접근할 경우 X-Frame-Options을 DENY로 설정하면 최신 브라우저에 접근할 수 없는 문제가 발생할 것으로 생각한다.

이 경우 동일 도메인에서는 iframe이 접근 가능하도록 X-Frame-Options를 SAMEORIGIN으로 설정하면 된다.

```
Apache
Header always append X-Frame-Options SAMEORIGIN
Nginx
add_header X-Frame-Options SAMEORIGIN;
```

위와 같이 설정하면 된다. 나 또한 위 설정을 통해 X-Frame-Options 설정을 변경했다. 그런데 문제가 해결되는 것이 아니라 또 다른 문제가 생겼다.

Multiple 'X-Frame-Options' headers with conflicting values ('SAMEORIGIN, DENY')
http 응답 헤더를 분석했더니 DENY와 더불어 또 하나의 SAMEORIGIN 설정이 추가되어 충돌이 발생하는 상황이었다. 아무리 DENY 설정을 제거하려고 삽질을 해봤지만 제거되지 않았다.

1시간 동안 Apache 웹 서버와 삽질하다가 혹시 Apache가 아니라 뒤에 연결되어 있는 Tomcat 서버의 응답 헤더에 DENY 설정이 포함되어 있는 것은 아닐까라는 의심을 하게 되었다.

> curl -I http://localhost:8080

위와 같이 명령을 실행해 응답 헤더를 확인하니 떡하니 DENY 설정이 추가되어 있다. 이럴 때의 허무함이란..

다시 Tomcat에서 X-Frame-Options 설정 시작했다. Tomcat은 기본 값으로 X-Frame-Options 설정을 DENY로 설정하고 있었다. 이 문제를 해결하기 위해 Servlet Filter를 추가해 해결했다.

```java
public class CorsFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        response.setHeader("X-Frame-Options", "ALLOW-FROM https://apps.facebook.com");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

위와 같이 설정하니 X-Frame-Options가 정상적으로 변경되는 것을 확인했다. chrome은 아직까지 ALLOW-FROM 설정을 인식하지 못해 무시해 버리기 때문에 정상적으로 동작한다. 다룬 브라우저도 정상적으로 동작... 오늘 오전의 삽질 경험을 공유해 본다.

## 참고 

https://gigas-blog.tistory.com/124

https://www.slipp.net/questions/402

## 출처

https://www.slipp.net/questions/402
