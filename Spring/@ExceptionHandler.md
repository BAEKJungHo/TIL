# @ExceptionHandler

@ExceptionHandler를 사용하면 자바에서 정의된 에러나 RunTimeException 등을 상속받아 구현한 커스텀 핸들러 등을 통한 ShowUserMessageException 등
에러가 발생했을때 @ExceptionHandler로 에러를 지정하여 응답을 만들어서 보낼 수 있다.

- REST API의 경우 응답 본문에 에러에 대한 정보를 담아주고, 상태코드를 설정하려면 ResponseEntity를 사용한다. 응답 본문에 담기기 때문에 클라이언트(자바스크립트)에서
responseText로 응답 본문에 담긴 에러 메시지를 출력할 수 있다.

- CustomException

```java
public class ShowUserMessageException extends RuntimeException {
}
```

```java
@Controller
@SessionAttributes("event)
@RequestMapping("/event")
public class EventController {

  /**
  * 에러 발생시 에러 메시지와 함께 특정한 페이지를 보여주고 싶은경우(에러 페이지, jsp가 있다고 가정)
  */
  @ExceptionHandler
  public String eventErrorHandler(ShowUserMessageException showUserMeesageException, Model model) {
    model.addAttribute("message", "event error");
    return "error";
  }
  
}
```  

- 여러개의 Exception을 처리하는 HandlerMethod

```java
  /**
  * 중괄호를 사용해 여러개의 Exception을 지정할 수 있다.
  * 매개변수에서는 여러개의 Exception을 포함하는 상위 Exception으로 설정해야한다.
  */
  @ExceptionHandler({ShowUserMessageExcpetion.class, RuntimeException.class})
  public String eventErrorHandler(RuntimeException exception, Model model) {
    model.addAttribute("message", "event error");
    return "error";
  }
```  

- REST API (ResponseEntity 사용하는 경우)

```java
return ResponseEntity.badRequest().body("error message");

// return ResponseEntity.ok("message");
```

이런식으로 응답본문에 적어줘야한다.


## Example

```java
@ControllerAdvice(annotations = {Controller.class, Service.class})
public class ExceptionHandler {

    @org.springframework.web.bind.annotation.ExceptionHandler(BusinessException.class)
    public String businessException(BusinessException e, HttpServletRequest request) {
        request.setAttribute("message_only", e.getMessage());
        return "weave/message";
    }
  
}
```

여기서 익셉션 메서드 파라미터에 Model 을 적어도 돌아가는 프로젝트가 있는 반면, Model 을 적게되면 아예 ExceptionHandler 자체가 안타는 경우도 있다. 원인은 아직 모름...

보통 Service 단에서 아래 처럼 에러를 날린다.

```java
try {
  // 생략
} catch(Exception e) {
  throw new BusinessException(e.getMessage()); 
}
```

e.getMessage 를 생성자로 넘기는 경우 만약 에러 내용에 `쿼테이션(', ")` 이 존재하는 경우 alert 을 출력하는 jsp 에서 에러가 발생할 수 있다. 

```html
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>메세지</title>
	<script type="text/javascript">
		<c:if test="${!empty message}">
			alert ('${message}');
			window.open("${pageContext.request.contextPath}/user/login.do", "_login", "location=no, scrollbars = yes, top=100, left=100, height=560, width=700");   
		</c:if>	
		
		<c:if test="${!empty message_only}">
			alert ('${message_only}');
		</c:if>	
		
		<c:choose>
			<c:when test="${empty forward}">
				window.history.back();
			</c:when>
			<c:otherwise>
				document.location.href = '${pageContext.request.contextPath}${forward}';
			</c:otherwise>
		</c:choose>			
	</script>		
</head> 
```

e.getMessage() 안에 쿼테이션이 들어있는 경우 F12 를 눌러 콘솔을 확인해보면 `Invalid or unexpected token` 에러가 발생한다.

전역 ExceptionhHandler 를 통해서 View 에 alert 을 띄우는 목적은 `에러가 발생했을 때 사용자에게 친절하게 에러 내용을 알려주기 위함`이므로 e.getMessage() 를 생성자로 넘기는것은 좋지 않다.
