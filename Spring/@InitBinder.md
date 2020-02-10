# @InitBinder

- 특정 컨트롤러에서 바인딩 또는 검증설정을 변경하고 싶을 때 사용

- 특정 모델 객체에만 바인딩 또는 Validator를 설정하고 싶은 경우

```java
@InitBinder("event")
```

## Example

- setDisallowedFields 를 사용한 블랙리스트 처리 기반
  - 받고싶지 않은 필드값을 걸러낼 수 있다.
  - 폼에서 setDisallowedFields에 설정한 필드를 보내도 객체에 바인딩 되지 않는다. 즉 null 값으로 박힌다.

```java
@InitBinder
public void setAllowedFields(WebDataBinder webDataBinder) {
  // id 값을 폼이나, 쿼리파라미터 등으로 받아오고 싶지 않은 경우
  // 즉, 폼에서 id값을 보내더라도 아래 설정이 있으면 객체에 바인딩 되지 않는다.
  webDataBinder.setDisallowedFields("id");
}
```

- setAllowedFields 화이트리스트 처리 기반 (입력 받고 싶은 필드들만 정의가능) 

- addCustomFormatter 

```java
public class Event {

  /**
  * DateTimeFormat.ISO.DATE : yyyy-MM-dd
  */
  @DateTimeFormat(iso = DateTimeFormat.ISO.DATE)
  private LocalDate startDate;
```

- webDataBinder.addValidators()
  - springFrameWork에서 지원하는 Validator 인터페이스를 상속받아서 구현

```java
public class EventValidator implements Validator {

  @Override
  public boolean suppors(Class<?> clazz) {
    // validator를 적용할 클래스가 어떤것인지를 적는다.
    // 아래는 Event 클래스를 Validation 할때 EventValidator를 사용한다고 정의하는 것임
    return Event.class.isAssignableFrom(clazz);
  }
  
  @Override
  public void validate(Object target, Errors errors) {
    Event event = (Event) target;
    if(event.id == null) {
      errors.rejectValue("id", "wrongValue", Message.DATE_ACCESS_ERROR.getMsg());
    }
  }

}
```

컨트롤러에서는 @InitBinder 어노테이션이 달려있는 메서드에 Validator 구현체를 등록하면 된다.

```java
@InitBinder
public void initBinder(WebDataBinder webDataBinder) {
  // id 값을 폼이나, 쿼리파라미터 등으로 받아오고 싶지 않은 경우
  // 즉, 폼에서 id값을 보내더라도 아래 설정이 있으면 객체에 바인딩 되지 않는다.
  webDataBinder.setDisallowedFields("id");
  // Validator 사용
  webDataBinder.addValidators(new EventValidator());
}
```

사실 위 처럼 인터페이스 구현 안하고 컨트롤러에서 private final EventValidator validator

validator.validate(event, bindingResult) 처럼 사용할 수 도 있다.


- 원하는 모델 객체에만 @InitBinder를 사용하고 싶으면 @InitBinder("모델 객체명")으로 사용할 수 있다.


따라서 @InitBinder를 잘 조합해서 사용하면 원하는 모델 객체 마다 `블랙리스트 또는 화이트리스트 및 Validator` 를 설정할 수 있다.
