# ObjectMapper readValue Error

- 우리는 객체를 직렬화, 역직렬화 하기 위해서 Jackson 라이브러리인 ObjectMapper 를 사용한다.
- 특히 JSON 형식 문자열을 REQUEST-BODY 에 담아서 REST API 요청을 하면, DTO 가 받아야한다.
  - 컨트롤러에는 `@RequestBody MemberRequest` 이런식으로 존재할 것이다.
- 스프링 부트는 다양한 메시지 컨버터를 제공하는데, `대상 클래스 타입`과 `미디어 타입` 둘을 체크해서 사용여부를 결정한다. 
- MappingJackson2HttpMessageConverter 를 사용하는 경우 DeSerializable(역직렬화) 하기 위해서 내부적으로 ObjectMapper 의 readValue 를 사용한다.
  - https://stackoverflow.com/questions/53191468/no-creators-like-default-construct-exist-cannot-deserialize-from-object-valu
- 만약에, DTO 에 `기본 생성자가 존재하지 않으면 아래와 같은 에러가 발생할 수 있다.
  - AbstractJackson2HttpMessageConverter 클래스의 readJavaType 메서드에서 발생하는 에러이다.
  -  ```
     Cannot construct instance of `nextstep.subway.applicaion.dto.StationRequest` (although at least one Creator exists): 
     cannot deserialize from Object value (no delegate- or property-based Creator)
     ```
