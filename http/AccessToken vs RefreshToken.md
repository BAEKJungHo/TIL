# AccessToken vs RefreshToken

- RefreshToken 의 핵심은 AccessToken 의 만료기간을 짧게 가져가서 보안상의 이점을 누리기 위함
- JWT 의 등장 배경은 세션 기반 인증 방식에서는, 인증을 위한 정보를 서버에 저장하고 인증할 때마다 DB 와의 통신을 했어야 했다. 매번 DB 와의 커넥션을 통해서 정보를 확인한다면 성능이 아무래도 좋지 않을 것이다.
따라서 JWT 는 인증 저장 방식을 서버가 아닌 클라이언트에게 위임하여 처리하도록 설계되었다. JWT 는 OAuth 에서 발급한 AccessToken 을 사용하는데, AccessToken 의 만료기간이 긴 상태에서 토큰을 탈취 당하면
만료 기간동안 개인정보를 해커에게 노출하게된다. 
따라서 이러한 보안상의 문제점을 해결하고자 `RefreshToken` 이 등장하게 되었다.
- RefreshToken 은 AccessToken 보다 만료 기간을 길게 잡고, RefreshToken 을 DB 에 보관하게되면 AccessToken 의 만료 기간을 짧게 가져가더라도 빈번한 로그인이 필요없어진다.
- AccessToken 이 만료되었으면 RefreshToken 을 인증 서버에게 보내 RefreshToken 을 확인하여 AccessToken 을 재 발급하는 형식이다.
- 따라서, JWT 의 방식과 세션 인증 방식의 장점을 혼합해서 사용한다고 보면된다. (물론 RefreshToken 자체를 DB 에 저장하기 때문에 완벽한 JWT 의 이점을 누릴 수는 없다.)