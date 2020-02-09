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
```

위에서 @ResponseBody를 붙이지 않은 이유는 ResponseEntity 자체가 응답이기 때문에 사용안해도 된다. 만약 반환 타입이 Resource였으면 @ResponseBody를
사용해야 한다.

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
