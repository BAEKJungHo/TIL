# AccessToken vs RefreshToken

- RefreshToken 의 핵심은 AccessToken 의 만료기간을 짧게 가져가서 보안상의 이점을 누리기 위함
- JWT 의 등장 배경은 세션 기반 인증 방식에서는, 인증을 위한 정보를 서버에 저장하고 인증할 때마다 DB 와의 통신을 했어야 했다. 매번 DB 와의 커넥션을 통해서 정보를 확인한다면 성능이 아무래도 좋지 않을 것이다. 따라서 JWT 는 인증 저장 방식을 서버가 아닌 클라이언트에게 위임하여 처리하도록 설계되었다. JWT 는 OAuth 에서 발급한 AccessToken 을 사용하는데, AccessToken 의 만료기간이 긴 상태에서 토큰을 탈취 당하면 만료 기간동안 개인정보를 해커에게 노출하게된다. 따라서 이러한 보안상의 문제점을 해결하고자 `RefreshToken` 이 등장하게 되었다.
  - Access token은 서버에 저장되지 않으므로(stateless) token이 탈취되면 서버에서는 막을 방법이 없다.
- RefreshToken 은 AccessToken 보다 만료 기간을 길게 잡고, RefreshToken 을 DB 에 보관하게되면 AccessToken 의 만료 기간을 짧게 가져가더라도 빈번한 로그인이 필요없어진다.
- AccessToken 이 만료되었으면 RefreshToken 을 인증 서버에게 보내 RefreshToken 을 확인하여 AccessToken 을 재 발급하는 형식이다.
- 따라서, JWT 의 방식과 세션 인증 방식의 장점을 혼합해서 사용한다고 보면된다. (물론 RefreshToken 자체를 DB 에 저장하기 때문에 완벽한 JWT 의 이점을 누릴 수는 없다.)

> [RFC-8725](https://datatracker.ietf.org/doc/html/rfc8725)

## 로그아웃 시 RefreshToken Redis 에서 삭제, 로그인 시 RefreshToken 과 AccessToken 발급

> refreshToken 이 redis 에서 제거되는거면, accessToken 만료시 refreshToken 으로 유효성을 체크할때 redis 데이터와의 일치 여부는 체크 안하는건가요?! 3분이 지나면 제거되는 이유가 궁금해요!

RefreshToken 의 등장 배경이,  AccessToken 의 만료기간이 긴 상태에서 탈취 당하게 되면 보안상 문제가 있기 때문에  AccessToken 의 만료기간을 짧게 가져가서 보안상의 이점을 누리기 위해서 등장한 것입니다. 따라서 RefreshToken 은 보통 클라이언트 쪽에서도 하나 가지고 있고, Redis 같은 In-memory DB 에서도 관리를 하는데, 클라이언트에서 AccessToken 보다 만료 시간이 긴 RefreshToken 을 탈취 당하게 되면, 해커는 RefreshToken 만료가 되기까지 계속 AccessToken 을 재발급 받을 수 있겠죠 ?ㅎㅎ

> 사실 3분은 짧긴 한 것 같은데,  3분이라는 정책은 회사마다 얼마든지 바뀔 수 있는 부분이고, 중요한 것은 아래 설명할 내용입니다. 

위와 같은 문제점 때문에 로그인할 때, 새로 RefreshToken 을 발급하고, 발급과 동시에 DB 에 저장하는 것이죠. 그리고 재발급시에 클라이언트에서 보낸 RefreshToken 이 DB 에 저장된 것과 동일한지 확인하는 거고요 따라서, 다른 환경에서 새로 로그인한 경우에는 기존 로그인 된 상태에서 관리되던 RefreshToken 은 만료기간이 남아있더라도 재사용할 수 없겠죠 ㅎㅎ

따라서, 첫 로그인 후, 혹은 로그아웃 시에 RefreshToken 을 제거하게 되면 해커가 AccessToken 을 재발급할 수 없겠죠 ! 

> refreshToken, accessToken 이 다른 redis 서버에 있는 이유가 궁금해요!

accessToken 은 Redis 에서 관리하진 않습니다. accessToken 은 클라이언트에서만 관리되는 토큰이고요, refreshToken 만 Redis 에 존재합니다.

## Front

![jwt2](https://user-images.githubusercontent.com/47518272/162605349-5bfa7dcf-14f8-4101-84c8-df20a85b4949.png)

- Access Token 만료가 될 때마다 계속 과정 9~11을 거칠 필요는 없다.
- 사용자(프론트엔드)에서 Access Token 의 Payload 를 통해 유효기간을 알 수 있다.
- 따라서 프론트엔드 단에서 API 요청 전에 토큰이 만료됐다면 바로 재발급 요청을 할 수도 있다.

## References

- https://tecoble.techcourse.co.kr/post/2021-10-20-refresh-token/
- https://tansfil.tistory.com/59
- https://curity.io/resources/learn/jwt-best-practices/#11-do-not-use-jwts-for-sessions
- https://sol-devlog.tistory.com/22
- [jwt-authentication-best-practices](https://blog.openreplay.com/jwt-authentication-best-practices)
- https://han-um.tistory.com/17
