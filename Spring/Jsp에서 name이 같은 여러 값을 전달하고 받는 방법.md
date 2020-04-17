# HttpServertRequest의 getParameterValues("name 명") 사용

```java
    xxxService.bulkInsert(
            request.getParameterValues("employeeDeptSeq"),
            request.getParameterValues("employeeInfoSeq"),
            request.getParameterValues("taskArray"),
            request.getParameterValues("positionArray"),
            request.getParameterValues("telArray"),
            request.getParameterValues("name"),
            request.getParameterValues("employeeId"),
            request.getParameterValues("emailArray")
    );
```

`request.getParameterValues('a');`

이렇게 한번에 받아 올 수 있다.

`String [] a = request.getParameterValues('a');` 

이렇게 받아오면 name 이 같은 값이 여러개일 경우 배열형태로 받아 올 수 있다.

## null 값이 넘어온 경우

input 태그의 name="name명" value 값이 지정 안되있고 null 인경우 `request.getParameterValues("name명")`으로 가져오면

문자열로 `"null"` 로 되어있다.
