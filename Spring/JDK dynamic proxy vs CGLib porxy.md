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

- test

```java
        ApplicationContext ac1 = new AnnotationConfigApplicationContext(TestConfig.class);
        TestConfig bean1 = ac1.getBean(TestConfig.class);
        System.out.println(bean1.getClass());

// 인터페이스를 빈으로 등록 할 수 없으니 에러 발생		
//        ApplicationContext ac2 = new AnnotationConfigApplicationContext(TestService.class);
//        TestService bean2 = ac2.getBean(TestService.class);
//        System.out.println(bean2.getClass());

// 1. 인터페이스를 구현한 경우
// 2. 인터페이스 구현 없이 @Service 로 사용한 경우
        ApplicationContext ac3 = new AnnotationConfigApplicationContext(TestServiceImpl.class);
        TestServiceImpl bean3 = ac3.getBean(TestServiceImpl.class);
        System.out.println(bean3.getClass());
```	

결과만 말하자면 인터페이스가 구현되어있는 경우 JDK dynamic proxy 를 사용하여 DI 하기 때문에 인터페이스를 선언해서 DI 받아야하고, 만약에 구현체로 선언해서 받고싶으면 CGLib-based proxy 를
사용하게 끔 설정하면된다.

@Congiguration 을 사용하여 빈으로 등록된애는 사실 CGLib 라이브러리를 생성해 @Configuration 을 적용한 클래스를 상속받은 새로운 클래스를 빈으로 등록하는 것이다.

```
class net.mayeye.site.config.TestConfig$$EnhancerBySpringCGLIB$$ff8bbd9f
```

@Service 와 같은 어노테이션으로 등록된 빈들은 인터페이스를 구현하든 아니면 클래스로 등록된 빈이던 아래와 같이 출력된다.

```
class net.mayeye.site.module.test.TestServiceImpl
```

아래 블로그에서는 JDK Dynamic Proxy는 인터페이스를 구현하여 Proxy를 생성해주고, Spring은 인터페이스가 아닌 클래스를 가지고 Proxy를 생성해주기 위해 CGLib 방식을 지원하고 있다고 하는데
CGLib 을 사용하면 첫 번째 처럼 뒤에 CGlib 이 찍혀야 하는건 아닐까? ...

## Reference

> https://gmoon92.github.io/spring/aop/2019/04/20/jdk-dynamic-proxy-and-cglib.html
