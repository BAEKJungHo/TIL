# @JsonProperty

- ObjectMapper 가 @JsonProperty 가 지정된 필드 이름으로 요청과 응답을 처리한다.

### Request

```
{
  "name" : "baekjh",
  "age" : "29",
  "email" : "qjxjfld13@gmail.com",
  "address" : "서울시",
  "phone_number" : "010-8924-1810"
}
```

### DTO

```kotlin
data class UserRequest(
    var name:String?=null,
    var age:Int?=null,
    var email:String?=null,
    var address:String?=null,

    @JsonProperty("phone_number")
    var phoneNumber:String?=null
)
```

### Response

```
{
  "name": "baekjh",
  "age": 29,
  "email": "qjxjfld13@gmail.com",
  "address": "서울시",
  "phone_number": "010-8924-1810"
}
```
