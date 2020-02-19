# @Validated

`javax.validation.Valid`를 통해 @Valid를 사용할 경우 그룹을 지정할 수 없다. 우리가 사용하는 @Min, @NotBlank 등은 그룹을 지정할 수 있는데 
해당 어노테이션을 사용해서 그룹을 지정하려면 @Validated를 사용해야한다.

단순하게 @Validated를 붙여도 @Valid와 동일한 효과가 난다.

## @Validated로 그룹지정하기

- domain class

```java
public class Event {

  interface ValidateLimit {}
  interface ValidateName {}
  
  @NotBlank(groups = ValidateName.class)
  private String name;
  
  @Min(value = 0, groups = ValidateLimit.class)
  private Integer limit;

}
```

- controller

```java
@Controller
@ReqeustMapping("/event")
public class EventController { 

  @GetMapping
  public String createEvent(@Validated(Event.ValidateLimit.class) @ModelAttribute Event event) {
 
    return "redirect:/event/form";
  }

}
```

`@Validated(Event.ValidateLimit.class)`를 통 ValidateLimit으로 지정한 그룹에 해당하는 애들만 유효성 검증을 실시한다.
