# ajax 비동기 서버와 통신할때 catch에 잡히는 이유

```javascript
  $.ajax({
      url: `${CONTEXT_PATH}/xxx/employee/api/edit`,
      data: JSON.stringify(params),
      method: 'POST',
      dataType: 'json',
      contentType:'application/json',
      success : function (res) {
          alert("저장되었습니다.");
      },
      error: function (xhr) {
          alert("실패하였습니다.");
      }
  });
```

보통 위처럼 선언해서 서버와 통신할때 

json 문자열 형식으로 보내는걸보니 서버에서는 @RequestBody 로 커맨드객체에 바인딩시켜주고

dataType 이 json 인걸보니 서버에서 응답본문으로 @ResponseBody 로 리턴할때 json 형식이어야한다. @ResponseBody 가 자바 객체를 json 형식으로 보내기 때문에 별다른 작업을 안해줘도 된다.

만약에 아래와 같이 빈 객체를 보낼 경우 null 혹은 json이 비어있어서 그런지, 브라우저 콘솔에 에러가 찍히지 않음에도 불구하고 catch로 떨어진다.
따라서 이를 방지하려면 실제 값이 들어있는 @RequestBody 어노테이션이 붙은 커맨드 객체를 리턴해주면 성공적으로 success 콜백 메서드를 탄다.

```java
public XXXVo ~ (@RequestBody xxxVo xxx) {

  return new XXXVo();
}
```
