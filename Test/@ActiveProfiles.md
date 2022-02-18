# @ActiveProfiles

```java 
@ActiveProfiles("test")
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@ExtendWith(RestDocumentationExtension.class)
public class Documentation {
    @LocalServerPort
    int port;

    protected RequestSpecification spec;

    @BeforeEach
    public void setUp(RestDocumentationContextProvider restDocumentation) {
        RestAssured.port = port;

        this.spec = new RequestSpecBuilder()
                .addFilter(documentationConfiguration(restDocumentation))
                .build();
    }
}
```

application.yml 은 하나 밖에 없는 경우에도 @ActiveProfiles("test")를 사용하는 경우가 있다.

전체 코드에서 현재 Profile 설정을 해둔 부분은 yml 파일 밖에 없고 클래스 쪽은 profile 에 따라서 다르게 동작하게끔 해둔 코드가 없는데, 굳이 테스트 코드를 test profile 로 돌려야 할까?

@ActiveProfiles("test") 를 왜 사용할까?


profile이 따로 설정되지 않았다면 application.yml 로 돌리게 되고 이 때의 profile 은 default profile 이다.
@ActiveProfiles("test") 를 떼면, 각 profile 설정값들을 사용하게 되면서 테스트에 영향을 미치게 된다.

__profile 설정에 영향을 받지 않고 돌아가게끔 하기 위해서 명시적으로 test profile 로 돌리는 것이다.__

## References

- https://bepoz-study-diary.tistory.com/371
