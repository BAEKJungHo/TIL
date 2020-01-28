# ResponseEntity에서 errorMessage를 던지기

보통 컨트롤러에서 xxxService.createXXX() 처럼 메서드를 호출하여 서비스 구현체에서 비지니스 로직을 처리한다.

serviceImpl에서 비지니스 로직을 처리하면서 에러가 발생할 경우 보통 아래처럼 에러를 던지는데

```java
  private void existsXXXPropertyValidator(xxxVo xxxVo) {
      if(organizationRepository.findExistsXXXProperty(xxxVo) != null) {
          throw new ShowUserMessageException("부서코드가 이미 존재합니다.");
      }
  }
```

해당 메시지를 받기 위해서는 컨트롤러 단에서 @ExceptionHandler 어노테이션을 사용하여 받을 수 있다.

```java
  @ExceptionHandler(ShowUserMessageException.class)
  public ResponseEntity showUserMessageExceptionHandler (ShowUserMessageException e) {
      return ResponseEntity.badRequest().body(e.getMessage());
  }
```    


그리고 해당 메시지를 js에서 아래처럼 사용할 수 있다.

```javascript
  .fail(function(err) {
    alert(err.responseText);
  });
```

즉, 에러 메시지는 responseText에 들어있다.
