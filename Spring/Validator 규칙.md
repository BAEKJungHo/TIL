# Validator 규칙

## @Valid 를 사용하는 경우 

- 등록, 수정, 삭제 로직을 처리함에 있어서 검증해야 하는 프로퍼티가 같은 경우

```java
public class EventVo {

    @NotBlank(message = "제목은 빈 값이 올 수 없습니다 ")
    private String title;
    
    @NotBlank(message = "내용은 빈 값이 올 수 없습니다.")
    private String content;
    
}
```

```java
@Controller
public class EventController {

    @PostMapping("/ins")
    public String ins(
        @Valid EventVo eventVo,
        BindingResult bindingResult
    ) {
        
        // 생략
    
    }

}
```

## @Validated 를 사용하는 경우

- 등록, 수정, 삭제 로직을 처리함에 있어서 검증해야 하는 프로퍼티가 서로 다른 경우 
- 하이버네이트 발리데이터 어노테이션에 그룹을 지정해서 사용
- 등록, 수정, 삭제가 검증에 같이 사용되는 프로퍼티인 경우  `{}` 로 구분
    - ex) 등록, 수정 로직 검증이 필요한 컬럼인 경우

```java
public class EventVo {

    public interface ValidateCreate {}
    public interface ValidateUpdate {}

    @NotBlank(message = "제목은 빈 값이 올 수 없습니다 ", groups = ValidateCreate)
    private String title;
    
    @NotBlank(message = "내용은 빈 값이 올 수 없습니다.", grops = {ValidateCreate, ValidateUpdate})
    private String content;
    
}
```

```java
@Controller
public class EventController {

    @PostMapping("/ins")
    public String ins(
        @Validated(EventVo.ValidateCreate.class) EventVo eventVo,
        BindingResult bindingResult
    ) {
        
        // 생략
    
    }

}
```

## 스프링 Validator 인터페이스 구현체를 만들어 사용하는 경우
    
```java
@Component
@RequiredArgsConstructor
public class UserCreateValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return User.class.isAssignableFrom(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        User user = (User) target;
        if (isBlank(user.getUserId())) {
            errors.rejectValue("userId", "required", "아이디를 입력해주세요.");
        }
    }
}
```

## 스프링 Validator 인터페이스 구현하지 않고, validate 메서드 만들어서 사용하는 경우

```java
@Component
@RequiredArgsConstructor
public class EmployeeInfoValidator {

    private final EmployeeInfoService employeeInfoService;

    public void validate(Employee employee, Errors errors) {
        if(isBlank(employee.getName()) {
            errors.rejectValue("name", "required", "이름을 입력해주세요.");
        }
    }

}
```
