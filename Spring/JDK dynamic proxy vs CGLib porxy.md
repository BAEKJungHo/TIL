# JDK dynamic proxy vs CGLib porxy

## Example

만약 인터페이스가 존재하고 해당 구현체가 존재하는데 다른 Service 에서 인터페이스로 DI 를 받는 것이 아닌 구현체를 DI 받는 경우

```java
@RequiredArgsConstructor
@Service
public class ArticleServiceImpl implements ArticleService {

  private final PrivacyServiceImpl privacyService;
  
  // 생략
  
}
```

서버 기동 시 다음과 같은 에러가 발생한다.

```
***************************
APPLICATION FAILED TO START
***************************

Description:

The bean 'mecPrivacyInfoServiceImpl' could not be injected as a 'net.weave.site.module.privacyInfo.service.PrivacyInfoServiceImpl' because it is a JDK dynamic proxy that implements:
	net.weave.site.module.privacyInfo.service.PrivacyInfoService


Action:

Consider injecting the bean as one of its interfaces or forcing the use of CGLib-based proxies by setting proxyTargetClass=true on @EnableAsync and/or @EnableCaching.
```

JDK dynamic proxy 와 CGlib proxy 에 대한 설명 글은 아래 링크 참고하면된다.

결과만 말하자면 인터페이스가 구현되어있는 경우 JDK dynamic proxy 를 사용하여 DI 하기 때문에 인터페이스를 선언해서 DI 받아야하고, 만약에 구현체로 선언해서 받고싶으면 CGLib-based proxy 를
사용하게 끔 설정하면된다.

## Reference

> https://gmoon92.github.io/spring/aop/2019/04/20/jdk-dynamic-proxy-and-cglib.html
