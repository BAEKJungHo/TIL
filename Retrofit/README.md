# Retrofit

> https://square.github.io/retrofit/
>
> https://square.github.io/okhttp/

## Retrofit 특징

- __기본적인 타임아웃 설정 값을 가지고 있다.__
  - https://jongmin92.github.io/2018/01/31/Programming/android-customize-network-timeouts/
  - RestTemplate 의 경우에는 Timeout 값을 따로 설정해줘야 한다.
- __BackEnd or FrontEnd 개발을 하다보면 다른 서버에 데이터를 요청할 일이 생긴다.__
  - Ex. PG 사에 결제 창 호출 요청 등
  - OPEN API 호출 등
- __Retrofit 은 `TypeSafe` 한 HttpClient 라이브러리이다.__
  - TypeSafe 하다는 의미는 네트워크로부터 전달된 데이터를 우리 프로그램에서 필요한 형태의 객체로 받을 수 있다는 의미이다.
- __Retrofit 은 `OKHttp` 에 의존하고 있다.__
- __Interface 에 기술된 HTTP API 의 구현체를 생성해 준다.__
  - BASE_URL : 요청할 서버의 기본 URL
  - getInstance()
    - setLenient() 설정이된 Gson() 객체를 만든다. 이는 Json 응답을 객체로 변환하기 위해 필요하다.
  - getApiService()
    - 앞서 구현한 getInstance() 메서드를 이용해, Retrofit 클라이언트를 생성한 뒤, Retrofit 클라이언트를 이용하여, Http API 명세가 담긴 Interface 의 구현체를 생성한 뒤 반환한다.

## HttpClient Library 의 사용 이유

Http 개발이 어렵기 때문이다. Http 를 개발한다면 아래의 것들을 고려해야 한다.

- 연결
- 캐싱
- 실패한 요청의 재시도
- 스레딩
- 응답 분석
- 오류 처리

이러한 것들을 개발하다보면 배보다 배꼽이 커진다.

## HttpURLConnection

HttpURLConnection 가장 원시적인 방법의 `HttpClient` 이다.

- __장점__
  - java.net 에 포함된 클래스로 별도의 라이브러리 추가가 필요 없다.
- __단점__
  - 자유도가 높은 대신, 직접 구현해야하는 것들이 많다.


