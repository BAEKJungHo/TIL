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

# @SpringBootTest

@SpringBootTest 는 통합 테스트 개념으로 모든 테스트를 등록해준다. 클래스 위에 @Component 로 빈으로 등록한 경우에는 web 과 관련된 설정이 아니기 때문에 @WebMvcTest 를 사용하여 테스트를 돌리면 테스트에 실패한다.

@SpringBootTest 를 사용하면 H2 데이터베이스를 자동으로 실행해 준다.

@SpringBootTest 를 사용할 때는, @AutoConfigureMockMvc 를 붙여야 의존성 주입이 가능하다.

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
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
