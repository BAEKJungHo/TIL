# RestAPI Guide

## Http 상태코드

## Http 메시지 

## @RequetBody 와 @ResponseBody

## 클라이언트와 서버의 비동기 통신

## ResponseEntity

- 스프링부트는 ObjectMapper 를 자동으로 사용, 스프링은 jackon 라이브러리를 넣어야 사용 가능
- 응답 코드와 바디에 값을 넣어서 보낼 수 있다.
- INTERNAL_SERVER_ERROR(500) 은 정말 심각할 때만 반환해야 하며, 보통 클라이언트에서 잘못된 값을 넘겨서 발생하는 경우 BAD_REQUEST(400) 을 던져야함
  - BAD_REQUEST 를 던지면 ajax 에서 error callback 을 탐
  - OK 를 던지면 ajax 에서 success callback 을 탐
- client -> server 로 거의 일반적으로 json 으로 보냄
	- @RequestBody String params -> id=baek&pw=123 이런 형식의 문자열로 담겨 있어서, json -> dto 로 파싱해야 함.
- ajax 에서 dataType : json 으로 설정하면, 서버에서 항상 ObjectMapper 를 사용하여 변경해줘야함(writeValueAsString), 클라이언트에서 받은 json 문자열을 읽으려면 readValue

## Exception

## DTO 를 사용해야 하는 이유

## Example

```java
@RequestMapping(value = "/validateCanLogin", method = RequestMethod.POST, produces = "application/json; charset=utf8")
public ResponseEntity validateCanLogin(@RequestBody String params) {
	String result = "";
	ObjectMapper objectMapper = new ObjectMapper();
	UserDto.ValidationLogin requestDto;
	try {
	    requestDto = objectMapper.readValue(params, UserDto.ValidationLogin.class);
	} catch(IOException e) {
	    throw new BusinessException(egovMessageSource.getMessage("fail.user.validateId"));
	}

	// 로그인이 가능한지 검증
	if(siteUserValidator.validateCanLogin(requestDto)) {
	  try {
		result = objectMapper.writeValueAsString(ValidationLoginResponse.SUCCESS_LOGIN);
	  } catch (Exception e) {
	    throw new BusinessException(Message.FAIL_LOGIN.getMsg()); 
	  } 
		return new ResponseEntity(result, HttpStatus.OK);
	}


	userVO.setUser_id(requestDto.getUser_id());
	int failCountCheck = userService.failCountCheck(userVO);
	UserVO idPwdCheck = userService.findUserByUserId(userVO);

	// 로그인 실패횟수 5번이면 임시비밀번호 발급 후 로그인 할 수 있도록 변경
	if(failCountCheck == 5) {
		try {
			result = objectMapper.writeValueAsString(ValidationLoginResponse.EXCEED_FAIL_COUNT);
			} catch (Exception e) {
			} 
		return new ResponseEntity(result, HttpStatus.OK);
	}

	// 로그인이 실패했을때 실패횟수 증가
	if(!requestDto.getPwd().equals(idPwdCheck.getPwd())) {
		userService.updateFailCount(userVO);
		try {
			result = objectMapper.writeValueAsString(ValidationLoginResponse.FAIL_LOGIN);
	} catch (Exception e) {
	} 
		return new ResponseEntity(result, HttpStatus.OK);
	}

	return new ResponseEntity(HttpStatus.BAD_REQUEST);
}

@org.springframework.web.bind.annotation.ExceptionHandler(BusinessException.class)
public ResponseEntity businessException(BusinessException e) {
	return new ResponseEntity(e.getMessage(), HttpStatus.BAD_REQUEST);
}
```

```javascript
var data = {'user_id' : user_id, 'pwd' : pwd};
$.ajax({
    url : url,
    type : 'post',
    data : JSON.stringify(data),
    datatype:'json',
    contentType:"application/json",
    success : function(res) {
    // HttpStatus.OK
	if(res === 'SUCCESS_LOGIN') {
	} 
    },
    error : function(err) {
    // HttpStatus.BAD_REQUEST
	alert(err.responseText);
	return false;
    }
});
```
