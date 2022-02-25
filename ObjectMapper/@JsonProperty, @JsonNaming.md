# @JsonProperty

- ObjectMapper 가 @JsonProperty 가 지정된 필드 이름으로 요청과 응답을 처리한다.
- 카카오, 네이버 등 외부 API 와 연동할 때 마다, DTO 를 만들 것이다.
  - 그 API 형식에 맞추기 위해 `@JsonProperty` 어노테이션을 붙이는 것은 매우 힘들다.
  - 따라서, @JsonProperty 대신 클래스 위에 `@JsonNaming` 어노테이션을 붙여서 사용할 수 있다.
  - 혹은 ObjectMapper 가 Bean 으로 변경할 때, 규칙을 정해준다던지, 스프링 application.properties 에 ObjectMapper 가 동작할 네이밍 규칙을 적어줄 수 있다.

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

# @JsonNaming

```kotlin
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy::class)
data class UserRequest(
    var name:String?=null,
    var age:Int?=null,
    var email:String?=null,
    var address:String?=null,
    var phoneNumber:String?=null
)
```
