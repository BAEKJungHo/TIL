# @Qualifier 로 구현체가 여러개인 인터페이스 의존성 주입 받기

만약, 동일한 타입을 가지는 빈 객체가 두 개 이상 있는 경우 스프링이 어떠한 빈을 주입해야하는지 알 수 없어, 스프링 컨테이너를 초기화 하는 과정에서 Exception 을 발생시킨다.

따라서 @Qualifier 어노테이션을 사용하면 실제 사용하기 위한 빈 객체를 선택할 수 있다.

- interface

```java
public interface MagicInterface {

  void basicSpell();
  
  ...
}
```

- impl

```java
// 구현체 Qualifier로 식별자를 "fire"로 지정
@Qualifier("fire")
public class FireMagic implements MagicInterface {
	...
}

// 구현체 Qualifier로 식별자를 "water"로 지정
@Qualifier("water")
public class WaterMagic implements MagicInterface {
	...
}

// 구현체 Qualifier로 식별자를 "earth"로 지정
@Qualifier("earth")
public class EarthMagic implements MagicInterface {
	...
}
```

- DI

```java
// Qulifier를 통해 서로 다른 구현체 주입
@Autowired
@Qualifier("fire")
private MagicInterface fireMagic;

@Autowired
@Qualifier("water")
private MagicInterface waterMagic;

@Autowired
@Qualifier("earth")
private MagicInterface earthMagic;
```

## References.

> https://velog.io/@owljoa/Spring-Boot-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85-%EC%8B%9C-%EC%84%9C%EB%A1%9C-%EB%8B%A4%EB%A5%B8-%EA%B5%AC%ED%98%84%EC%B2%B4-%EC%8B%9D%EB%B3%84-%EB%B0%A9%EB%B2%95
