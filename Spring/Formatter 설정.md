# WebMvcConfigurer Formatter 설정

- Formatter (아래 두 가지 인터페이스를 합친 인터페이스이다.)
  - Printer : 해당 객체를 (Locale 정보를 참고하여) 문자열로 어떻게 출력할 것인가 ? (객체 > 문자열)
  - Parser : 어떤 문자열을 (Locale 정보를 참고하여) 객체로 어떻게 변환할 것인가 ? (문자열 > 객체)
  
> 즉, 문자열을 객체로 바꿀 수 있고, 객체를 문자열로도 바꿀 수 있다. Locale 타입의 현재 지역정보도 제공

```java
  String print(T object, Locale locale); // OBJECT TO STRING
  T parse(String text, Locale locale) throws ParseException; // STRING TO OBJECT
```
  
- 추가방법 1
  - WebMvcConfigurer 의 addFormatters(FormatterRegistry) 메서드 정의
- 추가방법 2 (스프링 부트 사용시에만 가능)
  - 해당 포메터를 빈으로 등록
  
## @WebMvcTest

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

## Formatter Example

- 적용 X

```java
@RestController
public class SampleController {
  @GetMapping("/hello/{name}") 
  public String hello(@PathVariable("name") Person person) {
    return "hello" + person.getName();
  } 
}
```

현재는 위처럼 문자열 name 을 person 으로 받을 수 없다. 이를 해결하기 위해서는 Formatter 를 만들어야 한다.

- Formatter interface
  
```java
public interface Formatter<T> extends Printer<T>, Parser<T> { }
```

- Formatter 인터페이스 구현체 생성

```java
public class PersonFormatter implements Formatter<Person> {
  @Override
  public Person parse(String text, Locale locale) throws ParseException {
    Person person = new Person();
    person.setName(text);
    return person;
  }
  
  @Override
  public String print(Person object, Locale locale) {
    return object.toString();
  }
}
```

이렇게 formatter 를 사용하면 문자열을 객체로 받을 수 있다.
  
### 설정방법 1. 프로젝트 전역에 영향

- WebConfig.java 파일에 설정

```java
  @Override
  public void addFormatters(FormatterRegistry registry) {
      registry.addFormatter(new PersonFormatter());
  }
```    

### 설정방법 2. 해당 컨트롤러 내에서만 영향

```java
  @InitBinder
  public void personFormatter (WebDataBinder webDataBinder) {
      webDataBinder.addCustomFormatter(new PersonFormatter());
  }
```  
