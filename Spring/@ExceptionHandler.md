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
  @ExceptionHandler(
  public String eventErrorHandler(ShowUserMessageException showUserMeesageException, Model model) {
    model.addAttribute("message", "event error");
    return "error";
  }
  
}
```  
