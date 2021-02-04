# JSP 에서 현재 도메인 주소 가져오기

```html
<!-- http://localhost:8080/kor/bbs/BBSMSTR_123/lst.do -->
String requestUrl = (String) request.getAttribute("javax.servlet.forward.request_uri");
request.setAttribute("requestUrl", requestUrl);
```
