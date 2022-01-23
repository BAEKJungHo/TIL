# HttpMediaTypeNotAcceptableException

따로 ErrorResponse 객체를 만들어서 ResponseEntity 로 감싸서 리턴하는 경우 @Getter 가 없으면 `HttpMediaTypeNotAcceptableException` 에러가 발생한다.

- `ResponseEntity.ok().build()` : 정상
- `return new ResponseEntity<JsonResponse>(new JsonResponse(event), HttpStatus.OK)` : 정상
- ErrorResponse : 비정상
  - @Getter 없어서 그럼

- __JsonResponse 에 붙어있는 어노테이션__
  - @Getter, @Setter

@Setter 까지는 없어도 되고 @Getter 는 필요하다.

## References

- https://yuja-kong.tistory.com/entry/Spring-Boot-ExceptionHandler-%EC%A0%81%EC%9A%A9-%EC%8B%9C-HttpMediaTypeNotAcceptableException-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0
