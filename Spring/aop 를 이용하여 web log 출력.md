# aop 를 이용하여 web log 출력

```java
@Aspect
@Component
public class WebLogAspect {

    private Logger logger = Logger.getLogger(getClass());

    @Pointcut("execution(public * come.weave..repository.*.*(..))")
    public void webLog(){}

    @Around("webLog()")
    public Object doAround(ProceedingJoinPoint joinPoint) throws Throwable {
        // 스톱워치로 시간 측정
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();

        if (attributes != null) {
            HttpServletRequest request = attributes.getRequest();
            logger.info("URL : " + request.getRequestURL().toString());
            logger.info("URI : " + request.getRequestURI());
            logger.info("HTTP_METHOD : " + request.getMethod());
            // 추가적으로 IP, CLASS METHOD 를 설정할 수 있다.
            // 생략
        }
        
        Object returnValue = null;
        try{
            returnValue = joinPoint.proceed();
        }catch (DataAccessException e) {
            logger.error(joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName() + "exception occured " + e.getMessage(),e);
            throw e;
        }
        
        stopWatch.stop();
        logger.info(stopWatch.prettyPrint());
        logger.info("RETURN : " + returnValue);
        return returnValue;
    }
}
```
