# AOP

> 흩어진 관심사(중복되는 코드)를 한 곳에 모아서 처리

인터셉터는 컨트롤러 전 후에 처리할 코드를 모아두는 것이라 하면, AOP 는 주로 서비스(비지니스 로직)단에 있는 중복 코드를 한 곳에 모아서 처리할 때 주로 사용한다.

예를 들면, 문자발송 및 개인정보이력 쌓기 등이 있다.

## Example

```java
@Slf4j
@RequiredArgsConstructor
@Component
@Aspect
public class TestAOP {
  
  private final TestService testService;
  
  @Pointcut("execution(public * com.baekjh.core.module.test.service.TestServiceImpl.createTest(..))" +
    "|| execution(public * com.baekjh.core.module.test.service.TestServiceImp.updateTest(..))"
  public void createAOP() {}
  
  @Around("createAOP()")
  public Object doAround(ProceedingJoinPoint joinPoint) throws Throwable {
    if(joinPoint == null) { return null; }
    
    Object rawInfo = joinPoint.proceed();
    Object args = joinPoint.getArgs();
    Object vo = args[0];
    String signature = joinPoint.getSignature().toString();
    
    try {
        // 공통으로 처리할 비지니스 로직 코드 작성
    } catch (Exception e) {
      log.error("CREATE AOP FAILED")
    }
  }

}
```
