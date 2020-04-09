# 스프링 부트에서 Bean 등록 순서 결정하기

스프링과 스프링 부트는 ComponentScan 방식으로 @Component, @Service, @Controller, @Repository, @Bean, @Configuration 등 해당 어노테이션 붙은 
클래스들을 빈으로 등록한다.

스프링에서 xml 을 이용해서 bean 을 등록하게되면 자동적으로 위에서 아래로 bean 들을 스캔하여 생성 한다. 만약 A 클래스에서 B 빈을 참조하고 있는 경우 B 클래스의 위치가 A 보다 아래여도 스프링은 알아서 판단해서 순서를 바꿔서 참조한다.

반면, 스프링 부트는 패키지 순서대로 위에서 아래로 빈을 찾아 등록하는데 만약 패키지 구조가 아래와 같은 경우

- A
- B
- C

A 패키지에서 B 의 빈을 주입받아서 사용하려고 하면 에러가 발생한다.

## 해결방법 1 : @DependsOn

단점은 잘못 사용하면 무한루프에 걸릴 수 있다.

## 해결방법 2 : @PostConstruct

## 해결방법 3 : @Order

## References.

> https://jeong-pro.tistory.com/167
