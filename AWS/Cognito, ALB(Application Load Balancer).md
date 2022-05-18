# API 인증을 구성하는 몇가지 방안

- __ALB 사용__
- __Cognito 대신에 다른 SSO서비스인 Okta를 사용할 수 있음__
  - https://www.okta.com/resources/whitepaper/managing-access-legacy-web-apps/
  - https://www.okta.com/resources/webinar-developer-webinar-protecting-an-api-with-oauth/
- __ALB 대신에 Nginx(단 상용 plus버전)나 Haproxy 를 세울수도 있음__
  - https://m.blog.naver.com/f5networks_korea/221839153209
  - https://www.nginxplus.co.kr/?page_id=3598
  - https://www.haproxy.com/blog/using-haproxy-as-an-api-gateway-part-2-authentication/
- __Nginx(오픈소스) 확장 모듈로 검증부분만 구현하는 방법__
  - https://medium.com/@tumulr/building-an-api-gateway-with-nginx-lua-e3dff45e6e63
- __솔루션으로 직접 구성__
  - https://oingdaddy.tistory.com/198
  - https://calgary.tistory.com/57
- __코드로 필요한 부분만 직접 구현__
  - https://calgary.tistory.com/57

# ALB(Application Load Balancer)

- https://aws.amazon.com/ko/blogs/korea/built-in-authentication-in-alb/
- [Application Load Balancer를 사용하여 사용자 인증](https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/application/listener-authenticate-users.html)
- https://www.exampleloadbalancer.com/auth_detail.html

# Identity Provider

- https://www.okta.com/kr/identity-101/why-your-company-needs-an-identity-provider/

# Cognito

- [Amazon Cognito를 활용한 모바일 인증 및 보안, 자원 접근 제어 기법](https://www.youtube.com/watch?v=SiCQtRmvQBY)
- [Amazon Cognito 란?](https://docs.aws.amazon.com/ko_kr/cognito/latest/developerguide/what-is-amazon-cognito.html)
- [일반적인 Cognito 시나리오](https://docs.aws.amazon.com/ko_kr/cognito/latest/developerguide/cognito-scenarios.html)
- https://aws.amazon.com/ko/cognito/dev-resources/
- [Using the Cognito Hosted UI with an Application Load Balancer](https://www.kdgregory.com/index.php?page=aws.albCognito)

## 장단점

