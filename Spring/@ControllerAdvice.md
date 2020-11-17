# @ControllerAdvice

- 전역 컨트롤러 : @ControllerAdvice, @RestControllerAdvice
  - 예외 처리, 바인딩 설정, 모델 객체를 모든 컨트롤러 전반에 걸쳐 적용하고 싶은 경우에 사용한다.
  - @ExceptionHandler
  - @InitBinder
  - @ModelAttributes

- 적용할 범위를 지정할 수도 있다.
  - 특정 어노테이션을 가지고 있는 컨트롤러에만 적용하기(스프링 4.0 부터)
  - 특정 패키지 이하의 컨트롤러에만 적용하기
  - 특정 클래스 타입에만 적용하기
  
## Example 1

```java
@ControllerAdvice(annotations = {Controller.class, Service.class})
public class ExceptionHandler {

    @org.springframework.web.bind.annotation.ExceptionHandler(BusinessException.class)
    public String businessException(BusinessException e, HttpServletRequest request) {
        request.setAttribute("message", e.getMessage());
        return "egovframework/mayeye/message";
    }

}
```

## Example 2

```java
/**
* 모든 컨트롤러에 적용
* 특정 컨트롤러에 적용(클래스, 어노테이션, 패키지)
* - @ControllerAdvice(assignableTypes = {EventController.class, EventApi.class})
* - @ControllerAdvice(annotations = RestController.class)
* - @ControllerAdvice("org.example.controllers")
*/
@ControllerAdvice
public class BaseController {

  @ExceptionHandler({EventException.class, RuntimeException.class})
  public String eventErrorHanlder(RuntimeException ex, Model model) {
    model.addAttribute("message", "runtime error");
    return "error";
  }
  
  @InitBinder("event")
  public void initEventBinder(WebDataBinder webDataBinder) {
    webDataBinder.setDisallowedFields("id");
    webDataBinder.addValidators(new EventValidator());
  }
  
  @ModelAttribute
  public void categories(Model model) {
    model.addAttribute("categories", List.of("study", "seminar");
  }
 
}
```
