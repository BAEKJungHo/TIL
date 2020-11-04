# @Transactional 롤백 설정

`@Transactional(rollbackFor = Exception.class)`

- rollbackFor : 롤백이 수행되어야 하는 예외클래스를 적어준다

따로 설정을 안해주면 RuntimeException 에 대해서들만 롤백을 실행한다.
