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

- DB Access 를 통한 검증이 필요한 경우
    - ex) findXXX 로 어떠한 값을 조회하고 그 값이랑 비교하는 경우
    
```java
@Component
@RequiredArgsConstructor
public class MecAccntCreateValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return MecAccntVo.class.isAssignableFrom(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        MecAccntVo mecAccntVo = (MecAccntVo) target;
        boolean exists = accntService.existsAccnt(mecAccntVo);
        if (exists) {
            errors.rejectValue("admId", "overlap", "아이디가 중복됩니다.");
        }
        if (StringUtils.isBlank(mecAccntVo.getAdmId())) {
            errors.rejectValue("admId", "required", "아이디를 입력해주세요.");
        }
    }
}
```

```java
@Controller
@RequiredArgsConstructor
public class MecAccntController {

     private final MecAccntCreateValidator createValidator;
     
    @PostMapping("/ins")
    public String ins(
            MecAccntVo mecAccntVo
            ,BindingResult result
            ,RedirectAttributes redirectAttributes
    ) {
        createValidator.validate(mecAccntVo, result);
        beanValidator.showUserMessageBindingResultErrors(result);
        // 생략
    }
    
}
```

## 스프링 Validator 인터페이스 구현하지 않고, validate 메서드 만들어서 사용하는 경우

- DB Access 를 통한 검증이 필요한 경우
    - ex) findXXX 로 어떠한 값을 조회하고 그 값이랑 비교하는 경우

```java
@Component
@RequiredArgsConstructor
public class MecEmployeeInfoValidator {

    private final MecEmployeeInfoService employeeInfoService;

    public void validate(MecEmployeeInfoVo employeeInfoVo, Errors errors) {
        if(!employeeInfoService.duplicateValidator(employeeInfoVo)) {
            errors.rejectValue("notValid", "employeeId", "ID가 중복됩니다.");
        }
    }

}
```

## 컨트롤러에서 Errors 객체에 담긴 에러 처리 방법 

@Valid, @Validated, Validator 구현체, 커스텀 Validator 클래스를 통해 에러가 발생하면 Errors 객체에 담긴다.

컨트롤러에서 BindingResult 객체로 에러를 받을 수 있는데 BindgingResult 는 Errors 를 상속 받았기 때문이다.

```java
public interface BindingResult extends Errors {
    // 생략
}
```

- 처리 방법

```java
@Controller
@RequiredArgsConstructor
public class EventController {
    
    private final BeanValidator beanValidator;
    
    /**
     * 주의 !! 
     * BindingResult 파라미터는 커맨드 객체 바로뒤에 선언 되어 있어야 한다.
     */
    @PostMapping("/ins")
    public String ins(
        @Valid EventVo eventVo, 
        BindingResult bindingResult,
        RedirectAttributes redirectAttributes
    ) {
        beanValidator.showUserMessageBindingResultErrors(bindingResult); // 이 부분만 적어서 사용하면됨 
        eventService.createEvent(eventVo);
        redirectAttributes.addFlashAttribute("message", Message.CREATE.getMsg());
        return REDIRECT_URL;
    }
    
}
```

- BeanValidator 클래스

```java
@Component
public class BeanValidator {

    /**
     * @Valid, @Validated 어노테이션이 적용된 BindingResult 에러 메시지 출력
     * @param bindingResult
     */
    public void showUserMessageBindingResultErrors(BindingResult bindingResult) {
        if(bindingResult.hasErrors()) {
            for(ObjectError error : bindingResult.getAllErrors()) {
                throw new ShowUserMessageException(error.getDefaultMessage());
            }
        }
    }

}
```

## AuthorizationValidator

- 목적 
    - 권한과 관련된 검증 처리를 위한 Validator
    - 권한과 관련된 검증 로직은 여기에 모아두면 됨 

```java
@Component
public class AuthorizationValidator {

    public void hasWhiteListAuthorization(MecAccntVo authUser) {
        if(!MecWhiteList.WHITE_ACCNT_LIST.contains(authUser.getAdmId())) {
            throw new ShowUserMessageException("해당 권한은 최고관리자만 가능합니다.");
        }
    }

    public void hasWhiteListAuthorization(MecAccntVo authUser, String message) {
        if(!MecWhiteList.WHITE_ACCNT_LIST.contains(authUser.getAdmId())) {
            throw new ShowUserMessageException(message);
        }
    }

}
```

## CustomValidator Annotation

validations 패키지에 보면 Alphabet 이라던지 Number 등을 볼 수 있다.

VO 에서 @NotBlank 사용하는 것과 마찬가지로 영문만 입력 가능하게 하려면 `@Alphabet` 숫자만 입력 가능하게 하려면 `@Number` 를 사용하면 된다.

`@Date` 는 yyyy-MM-dd 형식의 날짜만 입력가능하게 해준다.

## ValidateUtils.requiredValueValidate()

어설션의 목적은 `의사소통과 디버깅`의 편리함을 위해서 사용되는 도구인데, Assert 를 if 문 대신해서 사용하는 것은 안됨. 

어설션은 특정조건이 참일 때만 코드의 일부가 실행되는 경우에 개발자간의 의사소통과 디버깅을 편리하게 하기 위해서 사용되는 것이다.

어설션을 사용한 경우 `// TODO Using Assertion` 이라는 주석을 넣어 어설션을 사용했다는 것을 표시해야 한다. 

어설션은 대게 제품화 단계에서 삭제한다.

어설션을 사용하면서 주의해야할 점이 있다. 어설션이 최종 실행환경에서는 빠져버리고 실행되지 않기 때문에 업무 로직에 해당하는 체크사항을 어설션을 사용해서 체크하게 되면 실행시에는 체크가 되지 않아서 문제가 발생할 수 있다. 개발환경과 실행환경의 프로그램이 달라지는것이다.

즉, 어설션은 코드의 전제조건을 나타내며 의사소통과 디버깅을 편리하게 해주지만, 절대 예외처리 용도로 사용 안된다.

- Before

```java
double getExpenseLimit() {
  // 비용 한도를 두든지, 주요 프로젝트를 정해야 한다.
  return (_expenseLimit != NULL_EXPENSE) ?
    _expenseLimit:
    _primaryProject.getMemberExpenseLimit();
}
```

- After

```java
double getExpenseLimit() {
  Assert.isTrue(_expenseLimit != NULL_EXPENSE || _primaryPorject != null);
  return (_expenseLimit != NULL_EXPENSE) ?
    _expenseLimit:
    _primaryProject.getMemberExpenseLimit();
}
```


기존(공주대 프로젝트까지만 하더라도)에 사용되고있던 Assert 를 보면 `의사소통과 디버깅` 편리함을 위함이 아닌 기본키와 같은 필 수값 검증 용도로 사용하고 있어서 

ValidateUtils.requiredValueValidate() 메서드를 따로 만들어 두었다.

```java
public class ValidationUtils {

    /**
     * 필수값 Validate - Integer version
     * @param requiredValue
     * @param message
     */
    public static void requiredValueValidate(Integer requiredValue, String message) {
        if(requiredValue == null) {
            throw new ShowUserMessageException(message);
        }
    }

    /**
     * 필수값 Validate - String version
     * @param requiredValue
     * @param message
     */
    public static void requiredValueValidate(String requiredValue, String message) {
        if(StringUtils.isBlank(requiredValue)) {
            throw new ShowUserMessageException(message);
        }
    }

}
```

```java
ValidationUtils.requiredValueValidate(articleMasterSeq, "게시판 마스터 키값은 빈 값이 올 수 없습니다.");
```

위 처럼 사용하면 된다.

따라서 기본키와 같은 필수 값에 대한 검증은 ValidationUtils.requiredValueValidate() 로 진행하고 어설션은 목적에 맞게 사용하길 바란다.

