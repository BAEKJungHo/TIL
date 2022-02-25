# @AssertTrue

```kotlin
data class UserRequest(

        @field:NotEmpty
        @field:Size(min = 2, max = 8, message = "은 2 ~ 8 자리 사이어야 합니다.")
        var name:String?=null,

        @field:PositiveOrZero // age >= 0
        var age:Int?=null,

        @field:Email
        var email:String?=null,

        @field:NotBlank
        var address:String?=null,

        @field:Pattern(regexp = "^\\d{2,3}-\\d{3,4}-\\d{4}\$")
        var phoneNumber:String?=null,

        var createdAt:String?=null // yyyy-MM-dd HH:mm:ss
) {
    // @AssertTrue or @AssertFalse 를 붙이면 Validation 이 일어날 때, 해당 어노테이션이 붙은 메서드를 참고하여 실행시킨다.
    // @AssertTrue : 해당 메서드의 결과가 true 인지 검증
    // @AssertFalse : 해당 메서드의 결과가 false 인지 검증
    @AssertTrue(message = "생성일자의 패턴은 yyyy-MM-dd HH:mm:ss 여야 합니다.")
    private fun isValidCreatedAt(): Boolean {
        return try {
            LocalDateTime.parse(this.createdAt, DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"))
            true
        } catch (e: Exception) {
            false
        }
    }
}
```
