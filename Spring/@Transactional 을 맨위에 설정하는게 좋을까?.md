# @Transactional 을 맨위에 설정하는게 좋을까?

```java
@Transcational(readOnly = true)
@Service
public class UserService {

  @Transcation
  public void saveUser(Long id) {
    // 생략
  }
  
  // 생략
}
```

위 처럼 사용하는게 좋을까 아니면, class 맨 위에 @Transcation 를 설정하고 각각 @Transcational(readOnly = true) 을 설정하는게 좋을까?

__벤더마다 다르게 가져가야 한다고 판단한다.__

그 이유는 다음과 같다.

MySQL 의 경우 @Transactional(readOnly = true) 이 설정되어있는 상태에서 CUD 를 호출하면 에러를 발생시키지만, H2 의 경우에는 그냥 커밋된다고 한다.

> http://wonwoo.ml/index.php/post/839

따라서 DB 벤더마다 다르게 가져가야 할 것 같다.

## @Transactional(readOnly = true) 의 성능 향상

스프링 부트에서 트랜젝션을 읽기 전용으로 설정할 수 있다.

`@Transactional(readOnly = true)`

class 명 위에 해당 어노테이션을 작성하면, Spring Boot 가 Hibernate Session flush mode 를 Manual 로 변경한다. 강제로(수동으로) flush 를 호출하지 않는 한 flush 가 일어나지 않는다.

트랜젝션을 commit 하더라도 영속성 컨텍스트가 flush를 일으키지 않아서, 엔티티의 등록, 수정, 삭제가 동작하지 않는다. 또한, 읽기 전용으로 영속성 컨텍스트의 변경 감지를 위한 snapshot 을 보관하지 않으므로 성능이 향상된다.

- __3줄 요약__
  - 트랜잭션이 끝나도 flush가 일어나지 않는다.
  - 엔티티 변경을 해도 동작하지 않는다. (강제로 flush하지 않는한)
  - 영속성 컨텍스트 변경감지를 위한 snapshot 이 필요없어지므로 성능이 향상된다.
