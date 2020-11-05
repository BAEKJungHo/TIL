# @Transactional 롤백 설정

- `@Transactional(rollbackFor = Exception.class)`
- `@Transactional(rollbackFor = {RuntimeException.class, Exception.class})`
- `@Transactional(noRollbackFor={IgnoreRollbackException.class})` : 해당 Exception 이 발생하면 Rollback 을 하지 않는다.

- rollbackFor : 롤백이 수행되어야 하는 예외클래스를 적어준다

스프링 프레임워크에서 `@Transactional` 어노테이션을 사용한 트랙잭션 처리에서 Checked 예외는 롤백 되지 않는다. 이것은 스프링 프레임워크가 EJB 에서의 관습을 따르기 때문이라고 합니다. 그러므로 기본적으로 Unchecked 예외는 롤백이 된다. RuntimeException 을 던지면 롤백이 된다는 것이다.

스프링이 JdbcTemplate 를 도입하면서 SQLException 을 DataAccessExcpetion(Unchecked Exception) 으로 던지기 때문에 쿼리에서 문제가 발생해서 터져도 롤백이 됬던 것이다. 

## 기본 설정

- 트랜잭션 전파 설정 : Propagation.REQUIRED (실행중인 트랜잭션 컨텍스트가 있으면 그 트랜잭션 내에서 실행하고, 없으면 새로 트랜잭션을 생성한다.)
- 트랜잭션 고립 레벨 : Isolation.DEFAULT (데이터베이스 설정을 따른다.)
- 읽기전용 : false (읽기/쓰기가 기본값이다.)
- 타임아웃 : -1 (타임아웃되지 않는다.)

> 예외에 따른 롤백처리는 Checked 예외는 롤백되지 않고, Unchecked 예외는 롤백된다.

## 롤백 기준

> https://pjh3749.tistory.com/269

## References.

> https://offbyone.tistory.com/405
