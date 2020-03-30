# HttpMessageConverter

- HTTP 메시지 컨버터
  - 요청 본문에서 메시지를 읽어들이거나(@RequestBody), 응답 본문에 메시지를 작성할 때(@ResponseBody) 사용한다.
  
- 기본 HTTP 메시지 컨버터
  - 바이트 배열 컨버터
  - 문자열 컨버터
  - Resource 컨버터
  - Form 컨버터(폼 데이터 to/form MultiValueMap<String, String>)
  - JAXB2 컨버터
  - Jackson2 컨버터
  - Jackson 컨버터
  - Gson 컨버터
  - Atom 컨버터
  - RSS 컨버터
  - ...
  
- 설정방법
  - 기본으로 등록해주는 컨버터에 새로운 컨버터 추가하기 : extendMessageConverters
  - 기본으로 등록해주는 컨버터는 다 무시하고 새로 컨버터 설정하기 : configureMessageConvertes
  - 의존성 추가로 컨버터 등록하기
    - 메이븐 또는 그래들 설정에 의존성을 추가하면 그에 따른 컨버터가 자동 등록된다.
    - WebMvcConfigurationSupport (이 기능 자체는 스프링 프레임워크 기능임, 스프링 부트 아님)

> HttpMessageConverter 를 어떤걸 적용해야 할지 판단하는 기준은, 스프링이 HTTP 헤더랑, contentType 을 가지고 판단한다.

## HttpMessageConverter : json

> 참고 : jsonPath 문법

## HttpMessageConverter : XML

- JacksonXML
- JAXB

> 스프링 부트는 기본으로 XML 의존성을 추가하지 않음

- 의존성 설정

```xml
<dependency>
  <groupId>javax.xml.bind</groupId>
  <artifactId>jaxb-api</artifactId>
</dependency>
<dependency>
  <groupId>org.glassfish.jaxb</groupId>
  <artifactId>jaxb-runtime</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-oxm</artifactId>
  <version>${spring-framework.version}</version>
</dependency>
```

- WebConfig 에 Marshaller 등록

```java
@Bean
public Jaxb2Marshaller marshaller() {
  Jaxb2Marshaller jaxb2Marshaller = new Jaxb2Marshaller();
  jaxb2Marshaller.setPackagesToScan(Event.class.getPackageName());
  return jaxb2Marshaller;
}
```

- 도메인 클래스에 `@XmlRootElement` 추가

> 참고 : Xpath 문법
