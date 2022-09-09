# ConstraintValidator

## Custom Validator Annotation 

```kotlin
@Target(AnnotationTarget.FIELD)
@MustBeDocumented
@Constraint(validatedBy = [NullableNotBlankValidator::class])
annotation class NullableNotBlank(
        val message: String = "{javax.validation.constraints.NotBlank.message}",
        val groups: Array<KClass<*>> = [],
        val payload: Array<KClass<out Payload>> = []
)
```

- message: ValidationMessages.properties에서 특정 property key를 가리키는 메시지 (제약 조건 위반시 메시지로 사용)
- groups: 유효성 검사가 어떤 상황에서 실행되는지 정의할 수 있는 매개 변수 그룹
- payload: 유효성 검사에 전달할 payload 를 정의할 수 있는 매개 변수

## Links

- https://gist.github.com/vbsteven/b2311c51b2686cbacf037b0fffb2eb06
- https://jongmin92.github.io/2019/11/21/Spring/bean-validation-2/
