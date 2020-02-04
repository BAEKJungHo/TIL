## ajax form 던지기

1. jQuery의 serializeObject 사용하기 : json 형식의 쿼리스트링으로 변환해서 보낸다.
2. new FormData() 사용하기

new FormData()를 log로 찍어도 보이지 key와 value값이 보이지 않는다.(https://stackoverflow.com/questions/17066875/how-to-inspect-formdata)

보는 방법은 아래와 같다.

```java
  const formData = new FormData(department.employeeForm);
  for (var key of formData.keys()) {
      console.log(key);
  }
  for (var value of formData.values()) {
      console.log(value);
  }
```

## serializeObject vs FormData

FormData 의 경우에는 주로 비동기 파일 업로드에 많이 쓰이는데, 그 이유는 serializeObject는 폼에 있는 input 데이터들을 긁어오면서 JSON 형식의
쿼리스트링으로 변환하기 때문에 input type file에 있는 binary data가 변환이 안된다. 

단순히 CRUD를 할 때에는 2가지는 크게 차이가 없다.

서버에서는 요청 헤더로 요청 데이터가 무슨 타입인지 판단하기 때문에 비동기로 보내도 폼 데이터 타입이면 html에서 폼 서브밋 한걸로 인식한다.

비동기 POST의 요청의 경우에는 `요청 본문`에 담겨오기 때문에 @RequestBody가 필수이며, 일반 GET 요청의 경우에는 데이터 보내는 일이 거의 없지만
쿼리스트링으로 오기 때문에 커맨드 객체만 사용해도 바로 바인딩이 된다.

## JSON parse error: Unrecognized token 'departmentSeq': was expecting ('true', 'false' or 'null');

> https://denodo1.tistory.com/322

- 프론트: JSON 데이터를 application/json으로 보내되, JSON 객체를 JSON.stringify()로 문자열화 해서 서버에 보내야 한다.
- 스프링 백엔드: JSON 데이터와 구조가 같은 DTO를 만들고, 컨트롤러 메서드에 @RequestBody를 붙여서 DTO를 파라미터에 추가하면 Spring이 Jackson을 통해 JSON의 값을 읽어서 DTO에 잘 넣어준다.
