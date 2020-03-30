# @WebMvcTest

이 어노테이션은 web 계층에서 사용되는 테스트용 어노테이션이다. 이 어노테이션을 사용하면 `MockMvc` 를 주입받을 수 있다.

```java
@RunWith(SpringRunner.class)
@WebMvcTest
public class SampleControllerTest {
  @Autowired
  MockMvc mockMvc;
  
  @Test
  public void hello() throws Exception {
    this.mockMvc.perform(get("/hello")) // get 으로 hello 라는 요청이 들어 왔을 때
      .andDo(print()) // 응답, 요청을 프린트
      .andExpect(content().string("hello")); // 해당 테스트 실행시 기대 되는(예상되는) 결과
  }
}
```
