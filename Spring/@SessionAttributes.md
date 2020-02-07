# @SessionAttributes

- 모델 정보를 HTTP 세션에 저장해준다.
- 이거대신 HttpSession을 이용해서 setAttribute로 HTTP 세션에 저장할 수 도 있다.
- @SessionAttributes를 사용하면 이 어노테이션을 설정한 이름에 해당하는 모델 정보를 자동으로 세션에 넣어준다.
  - 즉, @ModelAttribute를 HTTP Session에 담는다고 생각하면 된다. (단, 키값이 같은경우)
- @ModelAttribute는 세션에 있는 데이터도 바인딩한다.
- 여러 화면에서 사용해야하는 객체를 공유할 때 사용한다.
  - ex) 장바구니
  - ex) 폼에서 입력하는 값이 많은경우 화면을 나누기도하는데, 그때 A화면에서 input 값 a, b를 받고 그 다음 화면 B에서 c, d를 받아서 조합할 수 있다.

## SessionStatus

- SessionStatus를 사용해서 세션 처리 완료를 알려줄 수 있다.
- 폼 처리 끝나고 세션 비울때 사용한다.

## Example

```java
@Controller
@SessionAttributes("event")  
@RequestMapping("/events")
// @SessionAttributes({"event", "book"} 여러개도 지정가능
public class EventController {
  
  /**
  * @SessionAttributes 를 사용하여 모델에 담긴 event란 이름을 가진 객체를 세션에 담아준다.
  */
  @GetMapping("/form")
  public String eventsForm(Model model) {
    model.addAttribute("event, newEvent);
    return "/book/form";
  }
  
  /**
  * SessionStatus를 사용하여 폼 처리가 끝나고 세션을 완료시킬 수 있다.
  */
  @PostMapping
  public String createEvents(@Validated @ModelAttribute Event event,
                        BindingResult bindingResult,
                        SessionStatus sessionStatus) {
       if(bindingResult.hasErrors()) {
          return "/form";
       }
       sessionStatus.setComplete(); // 즉, 세션에 담긴값을 정리한다.
       return "redirct:/list";
   }
  
}
```

세션에 담긴 atttibute를 꺼내오려면 아래와 같이 하면 된다.

```java
Object event = request.getSession().getAttribute("event");
```
