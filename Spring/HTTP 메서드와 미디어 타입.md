# HTTP Method

- GET 요청
  - 클라이언트가 서버의 리소스를 요청할 때 사용
  - 캐시 사용 하는 경우 최종 결과가 캐시에 저장됨(캐싱 사용 가능)
  - 브라우저 기록에 남음
  - 북마크 가능
  - 민감한 데이터 보낼 때 사용 금지(URL 에 노출됨)
  - URL 데이터 길이 제한 약 2000 자 정도됨
  - idemponent

- POST 요청
  - 클라이언트가 서버의 리소스를 수정하거나 새로 만들 때 사용
  - 서버에 보내는 데이터를 POST 요청 본문에 담는다.
  - 캐시 불가능
  - 브라우저 기록에 남지 않음
  - 북마크 불가능
  - 데이터 길이 제한 없음
 
- PUT 요청
  - URI 에 해당하는 데이터를 새로 만들거나 수정할 때 사용한다.
  - POST 와 다른점은 URI 에 대한 의미가 다르다
    - POST 의 URI 는 보내는 데이터를 처리할 리소스 지칭
    - PUT 의 URI 는 보내는 데이터에 해당하는 리소스 지칭
  - idemponent 
  
- PATCH 요청
  - PUT 과 비슷하지만, 기존 에티티와 새 데이터의 차이점만 보낸다는 차이가 있다.
  - idemponent

- DELETE 요청
  - URI 에 해당하는 리소스를 삭제할 때 사용한다.
  - idemponent
  
## 컨텐츠 타입 맵핑

- 특정한 타입의 데이터를 담고 있는 요청만 처리하는 핸들러
  - @RequestMapping(consumes=MediaType.APPICATION_JSON_UTF8_VALUE)
  - Content-Type 헤더로 필터링
  
- 특정한 타입의 응답을 만드는 핸들러
  - @RequestMapping(produces=MediaType.APPICATION_JSON_UTF8_VALUE)
  - Accept 헤더로 필터링
  
> 문자열을 입력하는 대신 MediaType 을 사용하면 상수를 IDE 에서 자동완성으로 사용할 수 있다. 클래스에 선언한 @RequestMapping 에 사용한 것과 조합이
되지 않고 메서드에 사용한 @RequestMapping 의 설정으로 덮어쓴다.

### Example

```java
@RequestMapping(value = "/hello", consumes = MediaType.APPICATION_JSON_UTF8_VALUE)
@ResponseBody
public String hello() {
  return "hello";
}
```

## 헤더와 파라미터 맵핑

- 특정한 헤더가 있는 요청을 처리하고 싶은 경우
  - @RequestMapping(header = "key")
- 특정한 헤더가 없는 요청을 처리하고 싶은 경우
  - @RequestMapping(header = "!key")
- 특정한 헤더 키/값이 있는 요청을 처리하고 싶은 경우
  - @RequestMapping(header = "key=value")
- 특정한 요청 매개변수 키를 가지고 있는 요청을 처리하고 싶은 경우
  - @RequestMapping(params = "a")
- 특정한 매개변수가 없는 요청을 처리하고 싶은 경우
  - @RequestMapping(params = "!a")
- 특정한 요청 매개변수 키/값을 가지고 있는 요청을 처리하고 싶은 경우
  - @RequestMapping(params = "a=b")
  
### Example

```java
@RequestMapping(value = "/hello", headers = HttpHeaders.ACCEPT)
@ResponseBody
public String hello() {
  return "hello";
}
```

## HEAD 와 OPTIONS 요청 처리

우리가 구현하지 않아도 스프링 웹 MVC 에서 자동으로 처리하는 HTTP Method

- HEAD
- OPTIONS

- HEAD
  - GET 요청과 동일하지만 응답 본문을 받아오지 않고 응답 헤더만 받아온다.
- OPTIONS
  - 사용할 수 있는 HTTP Mehtod 제공
  - 서버 또는 특정 리소스가 제공하는 기능을 확인할 수 있다.
  - 서버는 Allow 응답 헤더에 사용할 수 있는 HTTP Method 목록을 제공해야 한다.
