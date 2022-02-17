# [REST] Stateless, JWT

![stateless](https://user-images.githubusercontent.com/47518272/154440721-143d5f57-a316-4eee-a361-0c5cc499c956.png)

## Stateless?

- 클라이언트에서 서버로 전송되는 각 요청에는 요청을 이해 하는 데 필요한 `모든 정보`가 포함되어야 하며 `서버에 저장된 컨텍스트를 이용할 수 없음`
- 요청 사이에 상태를 저장하지 않아도 서버 구성요소가 신속 하게 리소스를 확보할 수 있고, 서버가 요청 전반에 걸쳐 리소스 사용을 관리할 필요가 없기 때문에 `구현을 더욱 단순화` 할 수 있기 때문에 `확장성(Scalability)`이 향상

## 토큰 기반 인증

- 세션을 활용하는 Stateful 서버와는 달리 토큰을 활용하여 서버에 상태를 유지하지 않음
- 클라이언트 측에서 들어오는 요청의 정보로만 작업을 처리
- 서버와 클라이언트의 연결고리가 없기 때문에 확장성이 높아짐

## JWT

- JSON Web Token (JWT) 은 웹표준 (RFC 7519)
- 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인(self-contained) 방식으로 정보를 안전성 있게 전달

![jwt](https://user-images.githubusercontent.com/47518272/154441272-96d4254d-8a38-494b-a58a-9a79d910f1e8.png)

> 상세 설명은 링크 참조 https://velopert.com/2389

### JWT - 자가 수용적 (self-contained)

- JWT 는 필요한 모든 정보를 자체적으로 지니고 있음
- JWT 시스템에서 발급된 토큰은, 토큰에 대한 기본정보, 전달 할 정보, 그리고 토큰이 검증됐다는것을 증명해주는 signature 를 포함하고 있음

## References

- [JWT(JSON Web Token) 에 대해서](https://blog.outsider.ne.kr/1160)
- [RFC7519](https://tools.ietf.org/html/rfc7519)
