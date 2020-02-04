## ajax form 던지기

1. jQuery의 serializeObject 사용하기 -> json 형식의 쿼리스트링으로 변환해서 보낸다.
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
