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

## Repository 로 해결하기

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

## 새 클래스 작성

ABCService 와 XYZService 를 의존하는 새 클래스 QORService 를 작성해서 해결 할 수도 있다.

## 결론

테이블을 설계하고, 자바 클래스(entity, repository, service, controller 등)를 작성할 때, `테이블간의 의존성 관계를 고려하여 순환 참조가 일어나지 않게 빈 참조에 대해서도 신경써서 설계`해야 한다.

만약 repository 가 service 처럼 interafce, class 로 나뉘는 경우 더 신경 써야 할 것이다.

> ABCService 에서 만든 로직을 XYZService 에서 참조해서 사용해야 하고, ABCService 에서는 CRUD 를 처리하기 위해서 XYZ 테이블에 있는 값을 조회해서 얻어와야 하는 경우 XYZRepository 를 이용해야 한다. 만약 서로 각 service 에서 만든 로직을 서로 참조하게 설계한 경우, 잘못된 설계이다.

