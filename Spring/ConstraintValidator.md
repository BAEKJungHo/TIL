# ConstraintValidator 를 이용한 검증

- ConstraintValidator 의 장점
  - 일관성 있는 Validation 처리 방법
  - 일관성 있는 ErrorResponse
  
## ConstraintValidator 사용 방법

### 어노테이션 생성하기

```java
@Documented
@Constraint(validatedBy = OrderSheetFormValidator.class)
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface OrderSheetForm {
    String message() default "Order sheet form is invalid";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

### 검증 로직이 있는 ConstraintValidator 구현체 생성

> `ConstraintValidator<annotation interface, target class>`

```java
public class OrderSheetFormValidator implements ConstraintValidator<OrderSheetForm, OrderSheetRequest> { // (1)
    @Override
    public void initialize(OrderSheetForm constraintAnnotation) {
    }
    @Override
    public boolean isValid(OrderSheetRequest value, ConstraintValidatorContext context) {
        int invalidCount = 0; // (2)
        if (value.getPayment().hasPaymentInfo()) {
            addConstraintViolation(context, "카드 정보 혹은 계좌정보는 필수입니다.", "payment"); // (3)
            invalidCount += 1;
        }
        if (value.getPayment().getPaymentMethod() == PaymentMethod.CARD) {
            final Card card = value.getPayment().getCard();
            if (card == null) {
                addConstraintViolation(context, "카드 필수입니다.", "payment", "card");
            } else {
                if (StringUtils.isEmpty(card.getBrand())) {
                    addConstraintViolation(context, "카드 브렌드는 필수입니다.", "payment", "card", "brand");
                    invalidCount += 1;
                }
                if (StringUtils.isEmpty(card.getCsv())) {
                    addConstraintViolation(context, "CSV 값은 필수 입니다.", "payment", "card", "csv");
                    invalidCount += 1;
                }
                if (StringUtils.isEmpty(card.getNumber())) {
                    addConstraintViolation(context, "카드 번호는 필수 입니다.", "payment", "card", "number");
                    invalidCount += 1;
                }
            }
        }
        ...
        return invalidCount == 0; // (6)
    }
    private void addConstraintViolation(ConstraintValidatorContext context, String errorMessage,
        String firstNode, String secondNode, String thirdNode) {
        context.disableDefaultConstraintViolation(); // (4)
        context.buildConstraintViolationWithTemplate(errorMessage) // (5)
            .addPropertyNode(firstNode)
            .addPropertyNode(secondNode)
            .addPropertyNode(thirdNode)
            .addConstraintViolation();
    }
}
```

(1) ConstraintValidator<OrderSheetForm, OrderSheetRequest>를 상속받습니다. OrderSheetForm 작성한 위에서 생성한 어노테이션, OrderSheetRequest는 @RequestBody으로 받는 객체입니다.
(2) invalidCount는 검증이 실패할 때마다 증가할 카운트 변수입니다.
(3) addConstraintViolation 메서드를 통해서 에러 메시지와 검증한 node key 값을 넘겨줍니다. 해당 node는 ErrorResponse의 errors[].field에 바인딩 됩니다.
(4) 해당 메서드로 @OrderSheetForm의 default "Order sheet form is invalid"; 값을 disable 시킵니다.
(5) 해당 메서드로 검증에 대한 Violation 을 추가합니다.
(6) invalidCount == 0 아닌 경우에는 false

## 어노테이션 적용 클래스

```java
@NoArgsConstructor(access = AccessLevel.PRIVATE)
@Getter
@OrderSheetForm // (1)
public class OrderSheetRequest {
    @Min(1)
    private BigDecimal price;
    @NotNull
    @Valid // (2)
    private Payment payment;
    @NotNull
    @Valid
    private Address address;
    @NoArgsConstructor(access = AccessLevel.PRIVATE)
    @Getter
    @ToString // (3)
    public static class Payment {
        @NotNull
        private PaymentMethod paymentMethod;
        private Account account;
        private Card card;
        @JsonIgnore
        public boolean hasPaymentInfo() {
            return account != null && card != null;
        }
    }
    ...
}
```

(1) @OrderSheetForm을 추가해서 OrderSheetFormValidator가 동작하게 합니다.
(2) @Valid을 추가해서 각 클래스의 JSR-303 기반 어노테이션이 동작하게 합니다. @Valid이 없는 경우 payment.PaymentMethod의 @NotNull 동작하지 않습니다.
(3) Error[].value 값이 객체인 경우에 해당 객체의 정보를 출력하기 위해서 @ToString을 추가합니다.

### Controller

```java
@RestController
@RequestMapping("/order")
public class OrderApi {
    @PostMapping
    public OrderSheetRequest order(@RequestBody @Valid final OrderSheetRequest dto) {
        return dto;
    }
}
```

- @Valid 어노테이션으로 검증을 진행합니다.

## References.

> https://www.popit.kr/spring-%ea%b8%b0%eb%b0%98-constraintvalidator%ec%9d%84-%ec%9d%b4%ec%9a%a9%ed%95%b4%ec%84%9c-%ed%9a%a8%ea%b3%bc%ec%a0%81%ec%9d%b8-%ea%b2%80%ec%a6%9d/
