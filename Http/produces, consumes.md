# produces, consumes

- __consumes__
  - Content-Type 헤더에 consumes 로 지정한 타입이 들어있어야, 핸들러가 요청을 처리한다.
  - `consumes = MediaType.APPLICATION_JSON_UTF8_VALUE`
    - Content-Type이라는 HTTP 헤더에 `application/json;charset=UTF-8`(=MediaType.APPLICATION_JSON_UTF8_VALUE)라는 값이 있는 경우에만 처리를 한다.
- __produces__
  - 반환하는 데이터 타입을 정의한다.
  - `produces = MediaType.APPLICATION_JSON_VALUE`
