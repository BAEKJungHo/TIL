# BindingResult

```java
public String create(@Validated @ModelAttribute Event event, BindingResult bindingResult) {
  if(bindingResult.hasErrors()) {
    bindingResult.getAllErrors().forEach(error -> {
      System.out.println(String.valueOf(error));
    });
  }

  return "redirect:/list";
}
```

vo 내부에서 

```java
@NotBlank(groups = {ValidateCreate.class, ValidateUpdate.class}, message = "행사명은 빈 값이 올 수 없습니다.")
private String title;
```

위 처럼 어노테이션을 지정한 경우 message 저 값은 error 객체의 `defaultMessage` 값이다.  

- `오류 트레이스 + 에러 메시지` 출력 하고 싶은 경우
  - String.valueOf(error)
- `에러 메시지` 만 출력 하고 싶은 경우
  - error.getDefaultMessage()
  
```java
facilityApplyValidator.validate(facilityApplyVo);
if(bindingResult.hasErrors()) {
    bindingResult.getAllErrors().forEach(error -> {
        throw new ShowUserMessageException(error.getDefaultMessage());
    });
}
```        

validate 메서드 내부에서는 에러 발생 시 `throw new ShowUserMessageException("예약 시작시간은 종료시간을 넘을 수 없습니다.");` 처럼 
에러 메시지를 출력하게 할 수 있다.
