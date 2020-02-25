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

이렇게 받아오면 name이 같은 값이 여러개일 경우 배열형태로 받아 올 수 있다.
