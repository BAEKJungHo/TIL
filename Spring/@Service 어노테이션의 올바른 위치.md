# @Service 어노테이션의 올바른 위치

- __일부 개발자는 다음을 원하기 때문에 인터페이스 에 @Service 를 배치하기로 결정할 수 있습니다.__
  - 인터페이스가 서비스 수준 목적으로만 사용되어야 함을 명시적으로 보여줍니다.
  - 새로운 서비스 구현을 정의하고 시작하는 동안 자동으로 Spring Bean 으로 감지하도록 합니다.

```java
@Service
public interface AuthenticationService {
    boolean authenticate(String username, String password);
}
```

- 우리가 알아차렸듯이, AuthenticationService 는 이제 더 자기 설명적이 됩니다. 
- @Service 어노테이션은 개발자에게 데이터 액세스 계층이나 다른 계층이 아닌 비즈니스 계층 서비스에만 사용하도록 조언합니다.
- 일반적으로 괜찮지만 단점이 있습니다. 
- 인터페이스에 Spring 의 @Service 를 배치하여 추가 종속성을 만들고 인터페이스를 외부 라이브러리와 연결하게 됩니다.

다음으로 새 서비스 빈의 자동 감지를 테스트하기 위해 AuthenticationService 구현을 생성해 보겠습니다.

```java
public class InMemoryAuthenticationService implements AuthenticationService {

    @Override
    public boolean authenticate(String username, String password) {
        //...
    }
}
```

우리의 새로운 구현인 InMemoryAuthenticationService 에는 @Service 주석이 없다는 점에 주의해야 합니다. 
인터페이스인 AuthenticationService 에만 @Service 를 남겼습니다.

기본 Spring Boot 설정을 사용하여 Spring 컨텍스트를 실행해 보겠습니다.

```java
@SpringBootApplication
public class AuthApplication {

    @Autowired
    private AuthenticationService authService;

    public static void main(String[] args) {
        SpringApplication.run(AuthApplication.class, args);
    }
}
```

- 앱을 실행할 때 악명 높은 NoSuchBeanDefinitionException 이 발생하고 Spring 컨텍스트가 시작되지 않습니다.
- 따라서 인터페이스에 @Service 를 배치하는  것만으로는 Spring 구성 요소의 자동 감지에 충분하지 않습니다.

## 결론

[Spring 의 문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Service.html) 에는 구현 클래스에서 @Service 를 사용 하여 구성 요소 스캔에서 자동 감지할 수 있다고 명시되어 있습니다.

특히 인터페이스나 추상 클래스에 @Service 어노테이션 을 두는 것은 아무런 효과가 없으며 @Service 로 어노테이션이 붙은 경우 구성 요소 스캐닝에 의해 구체적인 클래스만 선택된다는 것을 알았습니다.

## References

- https://www.baeldung.com/spring-service-annotation-placement
