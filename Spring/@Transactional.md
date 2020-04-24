# @Transactional

- 트랜잭션 이란?
  - 일련의 작업을 한 단위로 묶고, 모든 작업은 한 번에 커밋되거나 롤백되어야 한다는 원자성을 부여하기 위함
- @Transactional 사용
  - 서로 다른 insert, update, delete 쿼리를 2번 이상 같은 비지니스로직의 메서드에서 호출하는 경우 사용해야한다.
