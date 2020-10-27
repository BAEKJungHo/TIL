# Error 화면 출력

```html
 <pre>
  Request URI:  ${pageContext.errorData.requestURI}
  Servlet Name: ${pageContext.errorData.servletName}
  Status Code:  ${pageContext.errorData.statusCode}
  Message:      ${pageContext.errorData.throwable}
  Stack Trace:
  <c:forEach var="st" items="${pageContext.errorData.throwable.stackTrace}">
    ${st}
  </c:forEach>
</pre>
```
