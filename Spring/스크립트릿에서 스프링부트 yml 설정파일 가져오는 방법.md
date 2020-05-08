## 스크립트릿에서 스프링부트 yml 설정파일 가져오는 방법

### spring environment

### ApplicationContext 사용

- 클래스는 빈으로 등록이 되어있어야 한다.

```html
<%@ page import="xxx.xxx.xxx.util.SecurityAES256" %>
<%

        ApplicationContext act = WebApplicationContextUtils.getRequiredWebApplicationContext(request.getSession().getServletContext());
        SecurityAES256 securityAES256 = (SecurityAES256) act.getBean("securityAES256");
        
%>
```
