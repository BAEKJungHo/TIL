# 순환 참조(Circular Reference)

- ABCServiceImpl 

```java
@RequriedArgsConstrcutor
@Service
public ABCServiceImpl implements ABCService {
  private final XYZServcie xyzService;
  
  // 생략
}
```

- XYZServiceImpl

```java
@RequriedArgsConstrcutor
@Service
public XYZServiceImpl implements XYZService {
  private final ABCServcie abcService;
  
  // 생략
}
```

이렇게 2개의 Impl 에서 서로 각각의 service 를 참조하고 있으면 아래와 같은 에러가 발생한다.

```
**************************
APPLICATION FAILED TO START
***************************

Description:

The dependencies of some of the beans in the application context form a cycle:

┌─────┐
|  aBCServiceImpl defined in file [/Users/andrew/workspace/spring-boot/target/classes/com/baekjh/demo/spring/circular/ABCServiceImpl.class]
↑     ↓
|  xYZServiceImpl defined in file [/Users/andrew/workspace/spring-boot/target/classes/com/baekjh/demo/spring/circular/XYZServiceImpl.class]
└─────┘
```

스프링에서 DI를 통해 생성자 주입을 할 경우, 잘못 설계하면 위 처럼 순환 참조 문제가 발생한다.

가장 좋은 방법은 `재설계` 하는 것이다.

ServiceImpl 은 @Service 로 빈으로 등록한 존재이기 때문에 순환 참조가 발생할 수 있는데, ABCServiceImpl 에서 XYZServiceImpl 에 있는 별다른 로직이 아닌
단순히 DB 에서 값을 조회해서 와야 하는경우 XYZServiceImpl 을 아래 처럼 Repository 로 의존성을 주입받으면 순환 참조 문제가 해결된다.

```java
@RequriedArgsConstrcutor
@Service
public XYZServiceImpl implements XYZService {
  private final ABCRepostiroy abcRepository;
  
  // 생략
}
```

