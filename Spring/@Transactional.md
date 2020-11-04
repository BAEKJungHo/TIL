# @Transactional

- 트랜잭션 이란?
  - 일련의 작업을 한 단위로 묶고, 모든 작업은 한 번에 커밋되거나 롤백되어야 한다는 원자성을 부여하기 위함
- @Transactional 사용
  - 서로 다른 insert, update, delete 쿼리를 2번 이상 같은 비지니스로직의 메서드에서 호출하는 경우 사용해야한다.
  
> 스프링에서는 트랜잭션 처리를 지원하는데 그중 어노테이션 방식으로 @Transactional을 선언하여 사용하는 방법이 일반적이며, `선언적 트랜잭션` 이라 부른다.

클래스, 메서드위에 @Transactional 이 추가되면, 이 클래스에 트랜잭션 기능이 적용된 프록시 객체가 생성된다.


이 프록시 객체는 @Transactional이 포함된 메소드가 호출 될 경우, `PlatformTransactionManager` 를 사용하여 트랜잭션을 시작하고, 정상 여부에 따라 Commit 또는 Rollback 한다.


## 트랜잭션의 성질

1. 트랜잭션의 성질

▶ 원자성(Atomicity)

 - 한 트랜잭션 내에서 실행한 작업들은 하나로 간주한다. 즉, 모두 성공 또는 모두 실패. 

▶ 일관성(Consistency)

 - 트랜잭션은 일관성 있는 데이타베이스 상태를 유지한다. (data integrity 만족 등.)

▶ 격리성(Isolation)

 - 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야한다.

▶ 지속성(Durability)

 - 트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.


## References.

> https://goddaehee.tistory.com/167

