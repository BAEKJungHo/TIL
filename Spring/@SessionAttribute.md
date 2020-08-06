# @SessionAttribute 

- HTTP 세션에 들어있는 값을 참조할 때 사용
- HttpSession을 사용할 때 비해 타입 컨버전을 자동으로 지원하기 때문에 편리함
- HTTP 세션에 값을 넣고 빼고싶은 경우는 HttpSession을 사용
- @SessionAttributes 와는 다르다
  - @SessionAttributes는 해당 컨트롤러 내에서만 작동
    - 즉, 해당 컨트롤러 안에서 다루는 특정 모델 객체를 세션에 넣고 공유할 때 사용
  - @SessionAttribute는 컨트롤러밖(인터셉터, 필터 등)에서 만들어준 세션 데이터에 접근할 때 사용
  
  사실 @SessionAttribute를 이용해서 @SessionAttributes로 HTTP Session 에 넣어준 값에 접근할 수 있지만 사실 그런 용도는 아니고 
  컨트롤러 내에서는 @SessionAttributes, SessionStatus, @ModelAttribute를 이용해서 다룬다.
  
 ## 웹 사이트에 방문했을때 방문한 시간을 기록 Example
 
 - 웹 사이트에 방문했을때 방문한 시간을 기록을 이용하면, 게임일 경우 2시간이상 과도한 게임할 경우 메시지를 뿌려준다던지 가능하다.
 
 예를들어 웹 사이트에 방문했을때 방문한 시간을 기록하는 인터셉터 VisitTimeInterceptor가 있다고 가정하겠다.
 
 ```java
 public class VisitTimeInterceptor implements HandlerInterceptor {
 
  @Override
  public boolean preHandle(HttpServletReqeust request, HttpServletResponse response) {
    HttpSession session = request.getSession();
    if(session.getAttribute("visitTime") == null) {
      session.setAttribute("visitTime", LocalDateTime.now());
    }
    return true;
  }
}
```

컨트롤러에서는 아래와 같이 세션에 담긴 visitTime을 가져올 수 있다.

```java
@GetMapping
public String getEvents(Model model, @SessionAttribute LocalateTime visitTime) {


}
```

HttpSession을 이용해서 꺼낼 수도 있는데 문제는 LocalDateTime이 아니라 Object로 나온다. 즉, 그러면 `Type Conversion` 이 일어나게된다. 하지만
@SessionAttribute를 이요하면 `Type Conversion` 을 자동으로 지원해준다.

```java
@GetMapping
public String getEvents(Model model, HttpSession httpSession) {
  Object visitTime = httpSession.getAttribute("visitTime");

}
```
