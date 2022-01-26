# REST Uniform Interface

## API 설계 시

- url 을 통하여 리소스를 가리키기
- 요청 시 method 를 통하여 행위를 결정하기
- 요청 시 accept header 를 통하여 응답받을 리소스의 표현을 결정하기
- 요청 시 content-type header 를 통하여 요청 시 보낼 데이터의 형식을 알려주기
- 응답 시 status code 를 통하여 응답 결과를 요약하여 알려주기

## Resource

- 리소스는 네트워크 환경에서 관리하는 모든 정보
- 예를 들어 문서, 이미지, 일반적인 서비스, 리소스들의 집합, 객체 등

### request

```java
GET /stations/1 HTTP/1.1
accept: application/json
host: localhost:57906
connection: Keep-Alive
user-agent: Apache-HttpClient/4.5.12 (Java/1.8.0_252)
accept-encoding: gzip,deflate
```

### response

```java
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Fri, 10 Jul 2020 09:38:40 GMT
Keep-Alive: timeout=60
Connection: keep-alive

{
    "createdDate": "2020-07-10T18:38:40.549",
    "modifiedDate": "2020-07-10T18:38:40.549",
    "id": 1,
    "name": "강남역"
}
```

### Resource 가 아니라 Representation

```
{
    "createdDate": "2020-07-10T18:38:40.549",
    "modifiedDate": "2020-07-10T18:38:40.549",
    "id": 1,
    "name": "강남역"
}
```

## Resource Naming

- 리소스에 접근할 URI 중 Path 부분
- Ex. https://localhost:8080/`stations/1`

### Path

- 데이터의 계층적인 형태로 구성이 됨(비계층적 구조는 query)
- 리소스를 식별하기 위해 제공됨

```
  foo://example.com:8042/over/there?name=ferret#nose
   \_/   \______________/\_________/ \_________/ \__/
    |           |            |            |        |
 scheme     authority       path        query   fragment
    |   _____________________|__
   / \ /                        \
   urn:example:animal:ferret:nose
```

> [rfc3986 - Syntax Components](https://datatracker.ietf.org/doc/html/rfc3986#section-3)

- __Path 형식__
  - 복수 리소스
    - /customers
  - 단수 리소스
    - /customers/{id}
  - 하위 리소스
    - /customers/{id}/account
    - /customers/{customer-id}/accounts/{account-id}

## 리소스 종류별 표현

- __document__
  - 객체 인스턴스와 유사한 단일 개념
  - 단수개의 리소스를 표현
    - /resources/{id}
    - /resources/{id}/sub-resources/{id}
- __collection__
  - 리소스의 디렉토리를 의미
  - 복수를 사용하여 표현
    - /resources
    - /resources/{id}/sub-resources
- __store__
  - 서버가 아닌 클라이언트가 관리하는 리소스
  - Document 와의 차이는 고유식별자가 없음
  - 리소스를 새로 생성하지 않음
  - 장바구니와 비슷한 개념
  - 복수를 사용하여 표현
    - /resources/{id}/playlists
- __controller__
  - 절차라는 개념의 리소스
  - 실행 가능한 함수와 유사
  - 매개 변수와 반환 값이 존재
  - 동사를 사용해도 좋음
    - /resources/{id}/checkout
    - /resources/{id}/sub-resources/{id}/play

## API 설계에 도움을 주는 문서

- 많은 사용자들이 이용하는 API 설계를 참고하기
  - https://developer.github.com/v3
- 가이드 참고하기
  - https://docs.microsoft.com/ko-kr/azure/architecture/best-practices/api-design
  - https://restfulapi.net/rest-api-design-tutorial-with-example
 
> [HTTP API Design Guide from Heroku Platform API](https://github.com/interagent/http-api-design)
