# ResponseEntity

- ResposneEntity
  - 응답 상태코드
  - 응답 헤더
  - 응답 본문
  
## ResonseEntity를 이용한 파일 다운로드

```java

// 파일을 읽어오기위한 클래스
pirvate final ResourceLoader resourceLoader;

@GetMapping("/file/{fileName}")
public ResponseEntity<Resource> fileDownLoad(@PathVariable String fileName) {
  // 여기서 classPath란 resource 바로 아래있는 파일을 나타냄
  Reousrce resource = resourceLoader.getResource("classpath:"+fileName); 
  return ResponseEntity.ok()
    .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; fileName=\"" + resourceLoader.getFileName() + "\")
    .header(HttpHeaders.CONTENT_TYPE, "image/jpg")
    .body(resource);
    
    // return new ResponseEntity<Resource>(resource, HttpStatus.CREATED); 처럼 보낼 수도 있다.
```

위에서 @ResponseBody를 붙이지 않은 이유는 ResponseEntity 자체가 응답이기 때문에 사용안해도 된다. 만약 반환 타입이 Resource였으면 @ResponseBody를 사용해야 한다.

>  .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; fileName=\"" + resourceLoader.getFileName() + "\")는 사용자가 파일을 다운로드 받을때
어떤 파일명으로 다운받을지를 설정할 수 있다.
>
> .header(HttpHeaders.CONTENT_TYPE, "image/jpg") 미디어 타입을 설정할 수 있다.

파일 미디어 타입을 모를때 알아 낼 수 있는 라이블러리가 있는데 Tika라는 라이브러리가 있다. 사용하려면 메이븐에 추가해야한다.

```java
Tika tika = new Tika();
String mediaType = tika.detect(파일객체);
```

위 처럼 사용하면 mediaType을 구할 수 있다.

## ResponseEntity 반환에 따른 JS 동작

> 스프링부트가 아닌 스프링에서 JSON 데이터를 받기위해선 jacson-databind 라이브러리가 있어야 하며 ObjectMapper 를 통해서  `{"nickname" : "ABC", ...} Key 와 Value 형태의 JSON 형식`을 DTO 에 매핑 시켜줄 수 있다.

```java
@RequestMapping(value = "/validateNickname.do", method = RequestMethod.POST, produces = "application/json; charset=utf8")
public ResponseEntity validateNickname(@RequestBody String params) throws SQLException {
    ObjectMapper objectMapper = new ObjectMapper();
    MemberInfoDto.ValidationNicknameRequest requestDto;
    try{
        requestDto = objectMapper.readValue(params, MemberInfoDto.ValidationNicknameRequest.class);
    } catch (IOException e) {
        throw new DataAccessException("닉네임 검증 중 오류가 발생하였습니다.");
    }
    return memberInfoService.validateNickname(requestDto) ? new ResponseEntity(HttpStatus.OK)
            : new ResponseEntity(HttpStatus.BAD_REQUEST);
}
```

HttpStatus.OK 면 AJAX 의 SUCCESS 메서드를 타게되며, HttpStatus.INTERNAL_SERVER_ERROR 이면 AJAX 의 ERROR 메서드를 타게 된다.
