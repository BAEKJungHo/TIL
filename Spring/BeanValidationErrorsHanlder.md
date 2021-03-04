```java
@Component
public class BeanValidationErrorsHandler {

    /**
     * @Valid, @Validated 어노테이션이 적용된 BindingResult 에러 메시지 출력 및
     * validator 검증 실패 시 errors 객체가 담겨있으면 메시지 출력
     * @param bindingResult
     */
    public void showUserMessage(BindingResult bindingResult) {
        if(bindingResult.hasErrors()) {
            for(ObjectError error : bindingResult.getAllErrors()) {
                throw new ShowUserMessageException(error.getDefaultMessage());
            }
        }
    }

}
```
