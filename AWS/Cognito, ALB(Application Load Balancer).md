# API 인증을 구성하는 몇가지 방안

- __ALB 사용__
- __Cognito 대신에 다른 SSO 서비스인 Okta 를 사용할 수 있음__
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

## 인증(AuthN)/인가(AuthR)

- __AuthN__
  - 정체를 확인
  - ID/PASSWORD
  - ID Card
  - Security token
- __AuthR__
  - 허용/거부의 규칙
  - IAM Policy
  - Security Group

## AuthN, AuthR 과 관련된 AWS 서비스

- AWS Directory Service
- AWS IAM
- Amazon Cognito
- Amazon API Gateway

> 모바일 개발을 위해서 전문적으로 만든 서비스가 Amazon Cognito 임

### AWS Directory Service

- 쉽게 Microsoft Active Directory 를 AWS 상에서 사용
- 3개의 디렉토리 타입
  - Microsoft AD
  - Simple AD
  - AD connector
- 온프레미스 AD 와 통합 가능
- 고용자를 위한 in-house 시스템의 계정 관리
- IAM 과 연계되어 AWS 콘솔에 대한 SSO
- Sharepoint, Amazon WorkMail 등 윈도우 기반 비지니스 서비스에 대한 authN 제어

### AWS IAM

- AWS 리소스에 접근할 수 있는 권한과 인증을 컨트롤
- AuthN: 누가 AWS 리소스를 사용할 수 있는가?
- AuthR: 인증 받은 사용자가 어떤 AWS 리소스에 접근할 수 있는가?
- EC2, Lambda 등의 AWS 리소스에 대한 접근 권한에 대한 제어
- 관리 콘솔이나 API 를 사용하는 사용자에 대해 IAM 유저/그룹을 통해 관리
- EC2 혹은 Lambda 로 부터 AWS 리소스로 접근하는 접근권을 제어

### AWS Cognito

- 모바일 웹/앱에 대한 사인인, 사인업, 유저 관리
- 소셜 auth 와 커스텀 auth 지원
- 여러 디바이스간의 유저 데이터 동기화 기능 제공
  - Cognito sync 기능을 통해 제공됨
- 모바일 네이티브 앱 혹은 자바스크립트 브라우저 앱을 위한 Temporary credential 발급으로 AWS 리소스에 접근 가능하게 함
- 만약 authN 이 필요하다면, 고객은 서드파티 아이디 제공자나(IDP) Cognito User Pools 를 사용 가능

### AWS API Gateway

- 쉽게 Restful 웹 API 를 생성/조작
- API 의 관리 용이
- API 리소스 접근 제어를 위해 IAM, 커스텀 Auth 람다함수, Cognito 유저 풀이 제공 됨
- JSON 기반의 웹 API 를 생성
- 백엔드 서비스들과 연동 가능
- 만약 authN 이 필요하다면, 고객은 IAM, Custom Authroizer(Lambda Function) 혹은 Cognito User Pools Authorizer 를 이용할 수 있다.

