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
- https://jonghyeok-dev.tistory.com/m/40
- [How to Set Up AWS Cognito Authentication with Serverless and NodeJS](https://www.freecodecamp.org/news/aws-cognito-authentication-with-serverless-and-nodejs/)

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

# 모바일 개발

![aws mobile](https://user-images.githubusercontent.com/47518272/169054964-5c4b6f8f-6313-4d56-9118-4ebb1bff0efc.png)

# 기존 인증 흐름

![aws fed](https://user-images.githubusercontent.com/47518272/169055684-ab6816e9-4117-488b-82e4-0e4de6400f47.png)

## Cognito Identity

![aws cog identity](https://user-images.githubusercontent.com/47518272/169056875-86487eab-90e2-4bd0-85df-a60db781244b.png)

## User Pools

> 간단히 말하면 사용자 데이터베이스. 과거에는 유저 데이터베이스를 만들어야 했으나, 모바일 개발을 한다면 굳이 유저 데이터베이스를 가지고 있을 필요가 없다.

![aws userpools](https://user-images.githubusercontent.com/47518272/169057575-5d00a7cf-b742-4b46-bf54-609eec612893.png)

- 유저 데이터에 대한 보안 관리 용이
- 확장성이 용이
- 유저 데이터베이스를 생성에 대한 리소스 없음

## 유저 시나리오

![aws user scenario](https://user-images.githubusercontent.com/47518272/169059217-fc6aa5f0-fc4a-4c9f-8c5e-95ea47f6edda.png)

![aws user scenario2](https://user-images.githubusercontent.com/47518272/169059570-5d878932-56ba-4d0b-9647-b91d9ff953b3.png)

## 확장된 관리 기능

![aws extensions](https://user-images.githubusercontent.com/47518272/169059839-62df0d6d-5b3b-434d-94bc-62db25a549cc.png)

## Cognito Sign-in 

![aws cognito signin](https://user-images.githubusercontent.com/47518272/169060266-524dac40-f143-4f89-9f36-077bd4972e45.png)

## Triggers - Lambda for pre-post process

![aws lamb trigger](https://user-images.githubusercontent.com/47518272/169061227-e6590c0b-a26b-4bf9-9f14-eff1431a6508.png)

User Pools 에는 Lambda 를 이용해서 Trigger 를 설정할 수 있다. 

sign-in or sign-up 을 한 다음, 유저에 특화된 기능(Ex. 환영 메시지)이 추가적으로 필요한 경우 사용할 수 있다. 

> 분석 기능도 있다.

![aws trigger](https://user-images.githubusercontent.com/47518272/169061149-e11ac3bd-d5bd-473a-b074-566b94b2a4a5.png)

## Cognito User Pools 와 API 게이트웨이

![cognito user pools](https://user-images.githubusercontent.com/47518272/169061693-2e132c0c-2c4d-4aa7-a67f-544a438e43c5.png)

## Custom Auth Flow

![custom auth flow](https://user-images.githubusercontent.com/47518272/169062126-436b7263-f314-4f1a-8c00-2ae28468fd25.png)

## 유저 상태 이해

![user state](https://user-images.githubusercontent.com/47518272/169062386-700a04ab-0fd0-46a4-88b3-dc59172aff1a.png)

## 이메일과 폰의 인증번호 전송

![email phone](https://user-images.githubusercontent.com/47518272/169062561-3f4c8de5-58ee-4b45-a92a-22e667b4218b.png)

## Amazon Cognito 사용자 풀에서 별명 사용하기

![cognito alias](https://user-images.githubusercontent.com/47518272/169062740-9b89d756-3765-4aee-862e-69f83cfe2ad9.png)

# API Gateway 동작 방식

- __장점__
  - API 응답을 저장하기 위한 관리형 캐시
  - CloudFront 를 통한 응답속도 개선 및 DDos 보호
  - iOS, Android 및 Javascript 를 위한 SDK 생성
  - API 정의를 위한 Swagger 지원
  - Request/Response 데이터 변환

![api 호출 흐름](https://user-images.githubusercontent.com/47518272/169063144-e496c2f5-d862-4cdf-bf2f-754bb75b3074.png)

![aws model](https://user-images.githubusercontent.com/47518272/169063269-0a6fc054-2b06-42ae-aaab-432587f568cf.png)

## 빌드, 배포, 복제 및 롤백

![aws build](https://user-images.githubusercontent.com/47518272/169063494-f2e80a57-6a50-47da-a8f6-3b69bf9751d7.png)

![api config](https://user-images.githubusercontent.com/47518272/169063719-5b78b3a9-957e-4941-80d9-2c86e5f3e8d4.png)

![api build](https://user-images.githubusercontent.com/47518272/169063819-a4e93392-ffc1-4440-b25b-b76665d99341.png)

# Cognito User Pools Token

![cognito user pools token](https://user-images.githubusercontent.com/47518272/169064200-72242d8c-3354-4459-abb8-5eb107b5fab2.png)

# Flow

![cognito user pools, api gateway](https://user-images.githubusercontent.com/47518272/169064622-44de1991-daab-467a-a52a-3c5f5dc44bf0.png)

![cognito user pools, api gateway2](https://user-images.githubusercontent.com/47518272/169064939-b4f5f1d1-baca-4c87-b97b-27cd016afccf.png)

![cognito user pools, api gateway3](https://user-images.githubusercontent.com/47518272/169065435-692dafa5-b926-4598-b37d-3fa4cb824959.png)









