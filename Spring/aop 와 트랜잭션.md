# aop 와 트랜잭션

마일리지 적립 차감 프로그램을 개발하다가 


마일리지 적립을 (인기글 채택, 댓글등록, 대댓글, 회원가입 등을 수행하는경우 마일리지 적립, 상품을 구매하는경우 마일리지 차감) `aop 와 전략패턴 + 추상팩토리패턴`을 사용해서 구현하려고 했다.

createUser (impl 클래스) 를 먼저 동작시키고 @AfterReturning 을 통해서 aop 를 호출하고 aop 메서드 내에서 전략패턴을 찾아서 updateMileage 를 수행하게 하는거였는데

createUser 가 되고나서 updateMileage 에서 UncheckedException 이 발생해도 User 가 이미 등록이 되어있었다. 즉, 트랜잭션이 하나로 묶이지 않았던 것이다.

propagation 속성들도 줘봤지만 여전히 안됬다.

해결법을 찾다가 [배민 트랜잭션](https://woowabros.github.io/experience/2019/01/29/exception-in-transaction.html) 관한 글을 보는데 한 클래스의 메서드에서 다른 클래스의 메서드를 호출하는경우
롤백이 된다는 문제였다.

이 부분을 적용해서 aop 를 사용안하고 createUser 에서 전략패턴을 통해 updateMileage 를 호출하니 트랜잭션이 하나로 묶였다.

