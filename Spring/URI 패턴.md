# URI 패턴

- @RequestMapping 패턴
  - `?` : 한글자
    - "/author/???" ... "/author/123"
  - `*` : 여러글자
    - "/author/*" ... "/author/baek"
  - `**` : 여러 패스
    - "/author/**" ... "/author/a/b
  - `/{name:정규식}`
    - {programSeq:[0-9]+}
    
> 패턴이 중복되는 경우 가장 구체적으로 매핑되는 핸들러를 선택한다.

