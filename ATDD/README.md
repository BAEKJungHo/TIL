# ATDD 

![IMAGES](./images/atddcycle.png)

![IMAGES](./images/tddcycle.png)

## 인수 테스트

- __인수 테스트__
  - 사용자의 관점에서 올바르게 작동하는지 테스트
  - __UserStroy 에서 Scenario 를 도출하고 요구사항들을 테스트 하여, 사용자 관점에서 올바르게 작동하는지 테스트__
  - 인수 조건은 기술(개발) 용어가 사용되지 않고 일반 사용자들이 이해할 수 있는 단어를 사용
  - 클라이언트가 의뢰했던 소프트웨어를 인수 받을 때, 미리 전달했던 요구사항이 충족되었는지를 확인하는 테스트
  - 인수 테스트는 소프트웨어가 기능적으로 어떤일을 해야 하는지 알려주고 확실하고 믿을 만한 원천 
  - 사용자 인수 테스트, 애자일에서의 인수 테스트 등
  - __자동화 된 테스트__
    - 인수 테스트는 수동으로도 가능하지만, 자동화되면 새로운 시스템 변경 사항이 이미 구현된 요구사항에 영향을 미치지 않는지를 확인하는 `회귀 테스트`로도 사용할 수 있음
    - NEXTSTEP 1주차 듣고 든 생각 -> 회귀 테스트가 가능한 이유가 뭘까?
      - 보통 UserStory -> Scenario 를 만들고 테스트를 함
      - __중요한 것은 Test Code 가 최대한 Production Code 와 결합하지 않도록 하는 것이 중요__
  - __시나리오 기반 테스트__
    - 도메인이나 기술에 대한 배경 지식이 없어도 이해할 수 있음
    - 요구사항을 명확하게 이해할 수 있음
  - __Black Box Test__
    - 인수 테스트는 Black Box 테스트 형식이다.
    - `Black Box Test` : 세부 구현에 영향을 받지 않게 구현하기 
    > 인수 테스트는 블랙 박스 테스트의 성격을 가지는게 좋다. 시스템 내부 코드를 가능한 직접 호출하지 말고 외부에서 요청하는 방식으로 검증하는 것을 추천
    
> [Acceptance Test Driven Development](https://mysoftwarequality.wordpress.com/2013/11/12/when-something-works-share-it/)  

## ATDD 란 ?

- 원래는 다양한 관점을 가진 팀원(기획, 개발, 테스트 등)들과 협업을 위한 애자일 방법 중 하나
- 다른 관점에서 원활한 커뮤니케이션 없이 논의를 한다면 서로 다른 결과물을 상상하여 작업을 진행할 수 있음
- 따라서 프로덕트 결과물이 나오는 시점에서야 이해하고 있던 내용이 다름을 인지하게 되는 경우 발생
- ATDD는 이러한 리스크를 사전에 방지하고자 기획 단계부터 인수 테스트를 통해 공통의 이해를 도모하여 프로젝트를 진행

> NEXT STEP ATDD 에서 다루지 않는 내용
> 
> - 고객/업무 분석가, 개발자, 테스터는 인수 테스트를 함께 만들어 낸다
> - 고객, 개발자, 테스터간의 협업은 개발 프로세스의 불필요한 반복을 줄인다
> - 3인 체제의 힘은 가장 훌륭한 이수 테스트를 생성할 수 있다는데 있다.
> - 이렇게 된다면 사용자 인수 테스트와 시스템 테스트 간의 차이점은 없다.

## TDD vs ATDD

- TDD는 개발자 영역을 다루며 시스템을 구성하는 유닛이나 모듈을 테스트 함
- TDD 설계 이슈는 특정 모듈이나 클래스가 인수 테스트 전체 또는 일부분을 통과할 수 있도록 책임을 할당
- TDD 와 ATDD 는 동일한 품질 목표를 갖고, 서로 상호 관계에 있다.
- 궁극적으로 TDD를 통해 특정 모듈이나 클래스를 담당하고 이 들이 모여 인수 테스트 전체 또는 일부분을 통과할 수 있도록 책임을 할당

## ATDD 개발 프로세스

- 인수 조건 정의
- 인수 테스트 작성
- 문서화
- 기능 구현
- 테스트 리팩터링

## ATDD 예시

- __인수 조건__
  - 인수 테스트가 충족해야하는 조건
  - 인수 조건을 표현하는 여러가지 포맷이 있음
    - 시나리오 기반 표현 방식
    - Given-When-Then
 
> [Acceptance Criteria: Purposes, Formats, and Best Practices](https://www.altexsoft.com/blog/business/acceptance-criteria-purposes-formats-and-best-practices/)

```java
Feature: 최단 경로 구하기

  Scenario: 지하철 최단 경로 조회
    Given 지하철역들이 등록되어 있다.
    And 지하철노선이 등록되어 있다.
    And 지하철노선에 지하철역들이 등록되어 있다.
    When 사용자는 출발역과 도착역의 최단 경로 조회를 요청한다.
    Then 사용자는 최단 경로의 역 정보를 응답받는다.
```

```
Feature: 지하철 노선 관리 기능

  Scenario: 지하철 노선 생성
    When 지하철 노선 생성을 요청 하면
    Then 지하철 노선 생성이 성공한다.
    
  ...
```

```java
/**
 * When 지하철 노선 생성을 요청 하면
 * Then 지하철 노선 생성이 성공한다.
 */
@DisplayName("지하철 노선 생성")
@Test
void createLine() {
}
```

> [Cucumber Gherkin Syntax](https://cucumber.io/docs/gherkin/)

### 문서화

- Spring Rest Docs 를 활용하여 API 문서화
- 문서화를 위해서는 Mock 서버 & DTO 정의가 필요
- 프론트엔드, 다른 백엔드 개발자와 병렬로 작업하는데 유리함
- 인수 테스트와는 별개로 API 테스트를 수행
- 다른 개발자들과 협업 시 커뮤니케이션에 큰 장점

## 객체지향 생활 체조

- 규칙 1: 한 메서드에 오직 한 단계의 들여쓰기(indent)만 한다.
- 규칙 2: else 예약어를 쓰지 않는다.
- 규칙 3: 모든 원시값과 문자열을 포장한다.
- 규칙 4: 한 줄에 점을 하나만 찍는다.
- 규칙 5: 줄여쓰지 않는다(축약 금지).
- 규칙 6: 모든 엔티티를 작게 유지한다.
- 규칙 7: 3개 이상의 인스턴스 변수를 가진 클래스를 쓰지 않는다.
- 규칙 8: 일급 콜렉션을 쓴다.
- 규칙 9: 게터/세터/프로퍼티를 쓰지 않는다.

> [Thoughtworks Anthology](https://github.com/BAEKJungHo/thoughtworks-anthology/blob/master/06.%20%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%20%EC%83%9D%ED%99%9C%EC%B2%B4%EC%A1%B0.md)

## 테스트 도구

- __DatabaseCleanup__
  - EntityManager 를 활용하여 @Entity 어노테이션을 활용하여 테이블을 조회
  - 해당 테이블들을 Truncate 함
  - 그리고 ID 자동 증가 숫자를 1로 복구 시킴

> [Rest Assured](https://rest-assured.io/)
>
> [SpringBoot Application Test](https://docs.spring.io/spring-boot/docs/2.2.6.RELEASE/reference/html/spring-boot-features.html#boot-features-testing-spring-boot-applications)
>
> [MockMvc vs RestAssured](https://tecoble.techcourse.co.kr/post/2020-08-19-rest-assured-vs-mock-mvc/)
>
> [JsonPath](https://github.com/json-path/JsonPath)

## Live Templates

- Settings/Preferences > Editor > Live Templates.
- 우측 Template Group 에서 other 하위에 새로운 Live Template 을 추가

1. 버튼 클릭 후 Live Template 선택
2. Abbreviation(단축어)와 Description을 입력
3. Template test 에 아래 코드를 복사
4. Define > Java > Statement 선택
5. 우측 Options에서 Reformat according to style 선택 (Shorten FQ names는 선택되어 있음)

#### Template test - 파라미터 없는 케이스

```java
// when
io.restassured.response.ExtractableResponse<io.restassured.response.Response> response = io.restassured.RestAssured
        .given().log().all()
        .when().$METHOD$("$URI$")
        .then().log().all().extract();

// then
org.assertj.core.api.Assertions.assertThat(response.statusCode()).isEqualTo(org.springframework.http.HttpStatus.$STATUS$.value());
```

#### Template test - 파라미터 있는 케이스

```java
// when
java.util.Map<String, String> params = new java.util.HashMap<>();
io.restassured.response.ExtractableResponse<io.restassured.response.Response> response = io.restassured.RestAssured
        .given().log().all()
        .body(params)
        .contentType(org.springframework.http.MediaType.APPLICATION_JSON_VALUE)
        .when().$METHOD$("$URI$")
        .then().log().all().extract();

// then
org.assertj.core.api.Assertions.assertThat(response.statusCode()).isEqualTo(org.springframework.http.HttpStatus.$STATUS$.value());
```

## AcceptanceTest

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class AcceptanceTest {
    @LocalServerPort
    int port;

    @Autowired
    private DatabaseCleanup databaseCleanup;

    @BeforeEach
    public void setUp() {
        RestAssured.port = port;
        databaseCleanup.execute();
    }
}
```

- 다른 도메인에 대한 시나리오 기반으로 테스트 진행할 때, 위 클래스를 상속 받아서 사용
- 실제 서버가 아닌 테스트를 위한 서버를 로드하도록 설정
- 실제 request 가 아닌 인수 테스트의 request 를 받기 위함
- @SpringBootTest 을 클래스에 붙여주면 테스트를 위한 웹 서버를 사용
- webEnvironment 설정을 통해 웹 서버의 환경을 지정

## 인수 테스트 객체 설정

- 테스트를 위한 서버에 요청을 보내기 위한 클라이언트 객체 설정
  - ex. MockMvc, RestAssured, WebTestClient
- 테스트의 성격에 맞는 클라이언트를 선택해야 함

# TDD

단위 테스트는 특정 단위(테스트 대상)가 의도한대로 작동하는지 검증

![IMAGES](./images/tdd.png)

## 단위란?

- 단위에 대한 정의는 하는 사람마다 다 다를 수 있음
- 소프트웨어 시스템의 작은 부분에 초점을 맞춘 저수준이라는 개념
- 다른 종류의 테스트보다 훨씬 빠르고 작음
- 단일 메서드에서 전체 클래스에 이르기 까지 다양함

```java
@DisplayName("구간 추가")
@Test
void addSection() {
    // given
    Station 강남역 = new Station("강남역");
    Station 잠실역 = new Station("잠실역");
    Station 역삼역 = new Station("역삼역");
    Line line = new Line("2호선", "green", 강남역, 잠실역, 10);

    // when
    line.addSection(잠실역, 역삼역, 5);

    // then
    assertThat(line.getStations()).containsExactlyElementsOf(Arrays.asList(강남역, 잠실역, 역삼역));
}
```

## 통합과 고립(Sociable and Solitary)

- 단위 테스트 작성 시 관계를 맺고있는 대상(협력 객체)이 있는 경우를 고려해야 함
- 협력 객체를 실제 객체로 사용하는지 Mock(가짜) 객체로 사용하는지에 따라 테스트 구현이 달라짐

> 통합 테스트의 통합(Integration)과는 다름
> 단위의 정의를 논하기 앞서 테스트하는 단위가 통합(Sociable)되어야 하는지 고립(Solitary)되어야 하는지 먼저 고려해야 함

![IMAGES](./images/sociable.png)

### Sociable Unit Test

```java
public class GraphTest {
    private Lines lines;
    ...
    @BeforeEach
    void setUp() {
        lines = new Lines(lines1, lines2, lines3);
    }

    @Test
    public void findPath() {
        Graph graph = new Graph(lines);
        List<Station> result = graph.findPath(station1, station3);
        assertThat(result.getStations().size).isEqualTo(3);
    }
}
```

### Solitary Unit Test (with Mock)

```java
public class GraphTest {
    private Lines lines;
    ...
    @BeforeEach
    void setUp() {
        lines = mock(Lines.class);
    }

    @Test
    public void findPath() {
        when(lines.getStation()).thenReturn(expecedStations)
        Graph graph = new Graph(lines);
        List<Station> result = graph.findPath(station1, station3);
        assertThat(result.getStations().size).isEqualTo(3);
        verify(lines).findStation(station1.getId());
    }
}
```

## Test Double

- 테스트 목적으로 실제 객체 대신 사용되는 모든 종류의 척도 객체에 대한 일반 용어
- 즉, 실제 (예 : 클래스, 모듈 또는 함수)를 가짜 버전으로 대체한다는 의미입니다.
- 가짜 버전은 실제와 같은 것처럼 보이고 (동일한 메소드 호출에 대한 답변) 단위 테스트 시작시 스스로 정의한 미리 준비된 답변으로 답변합니다.
- Test Double에는 종류가 여러가지가 있지만 대체 한다는 큰 의미에서는 같다고 할 수 있음

![IMAGES](./images/testdouble.png)

## Stubbing

- 테스트 동안 호출이 되면 미리 지정된 답변으로 응답
- 미리 프로그램된 것 외의 것에 대해서는 응답하지 않음

![IMAGES](./images/stubbing.png)

### Mockito 활용

```java
@DisplayName("단위 테스트 - mockito를 활용한 가짜 협력 객체 사용")
public class MockitoTest {
    @Test
    void findAllLines() {
        // given
        LineRepository lineRepository = mock(LineRepository.class);
        StationRepository stationRepository = mock(StationRepository.class);

        when(lineRepository.findAll()).thenReturn(Lists.newArrayList(new Line()));
        LineService lineService = new LineService(lineRepository, stationRepository);

        // when
        List<LineResponse> responses = lineService.findAllLines();

        // then
        assertThat(responses).hasSize(1);
    }
}
```

### MockitoExtension 활용

```java
@DisplayName("단위 테스트 - mockito의 MockitoExtension을 활용한 가짜 협력 객체 사용")
@ExtendWith(MockitoExtension.class)
public class MockitoExtensionTest {
    @Mock
    private LineRepository lineRepository;
    @Mock
    private StationRepository stationRepository;

    @Test
    void findAllLines() {
        // given
        when(lineRepository.findAll()).thenReturn(Lists.newArrayList(new Line()));
        LineService lineService = new LineService(lineRepository, stationRepository);

        // when
        List<LineResponse> responses = lineService.findAllLines();

        // then
        assertThat(responses).hasSize(1);
    }
}
```

### Spring 을 활용한 Stubbing

```java
@DisplayName("단위 테스트 - SpringExtension을 활용한 가짜 협력 객체 사용")
@ExtendWith(SpringExtension.class)
public class SpringExtensionTest {
    @MockBean
    private LineRepository lineRepository;
    @MockBean
    private StationRepository stationRepository;

    @Test
    void findAllLines() {
        // given
        when(lineRepository.findAll()).thenReturn(Lists.newArrayList(new Line()));
        LineService lineService = new LineService(lineRepository, stationRepository);

        // when
        List<LineResponse> responses = lineService.findAllLines();

        // then
        assertThat(responses).hasSize(1);
    }
}
```

### 협력 객체, 테스트 더블은 언제?

> 원래 stub을 하려면 세부 구현에 의존할 수 밖에 없는 것인가요?

- 네 맞습니다.
- 테스트 더블을 사용 할 경우 테스트에서 협력 객체의 세부 구현을 알아야 합니다.
- __테스트 대상이 협력 객체를 가질 때__
  - 실제 객체를 사용 하면 협력 객체의 행위를 협력 객체 스스로가 정의
  - 가짜 객체를 사용 하면 협력 객체의 행위를 테스트가 정의
- __가짜 객체 특징__
  - 가짜 객체를 사용 할 경우 테스트 대상을 검증할 때 외부 요인(협력 객체)으로 부터 철저히 격리
  - 하지만 테스트가 협력 객체의 상세 구현을 알아야 함
- __실제 객체 특징__
  - 실제 객체를 사용 할 경우 협력 객체의 상세 구현에 대해서 알 필요가 없음
  - 하지만 협력 객체의 정상 동작 여부에 영향을 받음
- __테스트 코드를 작성 할 때__
  - 가짜 객체를 활용하면 실제 객체를 사용할 때 보다 조금 더 편하게 테스트를 작성할 수 있음
  - 하지만 상세 구현에 의존하는 테스트가 될 수 있음
- __추천하는 방법__
  - 우선 TDD 를 연습할 때 가급적이면 실제 객체를 활용하는 것을 우선으로 진행
  - 테스트 작성이 어렵거나 흐름이 잘 이어지지 않는다면 테스트 더블을 활용하는 방법으로 접근하시는 것을 추천

## 통합 테스트란?

- 우리가 만드는 대부분의 애플리케이션은 데이터베이스, 파일 시스템, 외부 라이브러리 등과 같이 다른 애플리케이션과 통합되어 개발하는 경우가 많음
- 적은 비용과 빠른 테스트를 위해 단위 테스트를 주로 사용하지만 실제로 상호 작용하는 내용에 대한 검증도 반드시 필요함
- 예를 들면 데이터베이스와의 통합을 테스트하는 경우 테스트를 실행할 때 데이터베이스를 실행해야함

![IMAGES](./images/itest.png)

- __테스트 필요성__
  - 외부 라이브러리 기능에 대해 검증 할 필요는 없음
  - 단, 그 부분을 사용하는 부분에 대한 검증이 필요
- __변경할 수 없는 코드 검증 시 실제 객체 사용__
  - 외부 라이브러리 코드의 작동 원리를 깊이 있게 이해하기 쉽지 않음
  - 목 객체의 행위와 실제 객체의 행위를 일치 시키기 쉽지 않을 수 있음

```java
@Test
void findPath() {
    // graphService에서 활용하는 Jgrpah라이브러리의 객체를 실제 객체로 사용한다.
    List<Long> stationIds = graphService.findPath(
            Lists.list(line1, line2), 
            station3.getId(), 
            station5.getId(), 
            PathType.DISTANCE
    );

    assertThat(stationIds).hasSize(5);
    assertThat(stationIds.get(0)).isEqualTo(3L);
    assertThat(stationIds.get(1)).isEqualTo(2L);
    assertThat(stationIds.get(2)).isEqualTo(1L);
    assertThat(stationIds.get(3)).isEqualTo(4L);
    assertThat(stationIds.get(4)).isEqualTo(5L);
}
```

### 예시 - 데이터 베이스

- 실제는 다른 DB를 사용하지만 테스트를 쉽게 실행할 수 있도록 테스트는 메모리 내 H2 데이터베이스에 연결
- 결국 통합 테스트는 프로덕션 환경과 다른 유형의 데이터베이스에 대해 실행됨

```java
@DataJdbcTest
public class StationRepositoryTest {
    @Autowired
    private StationRepository stationRepository;

    @Test
    void saveStation() {
        String stationName = "강남역";
        stationRepository.save(new Station(stationName));

        assertThrows(DbActionExecutionException.class, () -> 
            stationRepository.save(new Station(stationName))
        );
    }
}
```

### 통합 테스트 vs 단위 테스트

- 통합 테스트는 통합하고 있는 부분을 실제와 가까운 환경에서 검증하여 기능이 정상적으로 여부를 검증
- 단위 테스트는 통합하고 있는 부분이 정상적으로 동작한다고 가정하고 단일 기능에 대해서만 검증

## 학습 테스트 실습

- 학습 테스트를 통해 단위 테스트 기능 익히기

### UnitTest

- 객체의 단위 테스트를 작성
- 협력 객체를 사용한 단위 테스트를 작성

### MockitoTest

- mockito 를 활용하여 가짜 협력 객체를 사용한 단위 테스트 작성

### MockitoExtensionTest

- mockito 의 MockitoExtension 을 활용하여 가짜 협력 객체를 사용한 단위 테스트 작성

### SpringExtensionTest

- SpringExtension 을 활용하여 가짜 협력 객체를 사용한 단위 테스트 작성

### JGraphTest

= 경로 탐색을 위한 라이브러리 JGraph의 학습 테스트

## ATDD 이후 사이클

- TDD Cycle 에 따라 기능을 구현
- 지키기 어려운 것
  - 테스트 코드가 검증하는 만큼만 구현하기
  - 프로덕션 코드보다 테스트 코드 먼저 고치기

## Outside-In TDD

- 인터페이스를 만들면서 테스트를 작성함으로써 개발 시작
- 협력 객체에 대해서는 예측하여 테스트 할 대상과 협력객체들 사이의 상호작용을 고려하여 진행
- 테스트가 성공하면, Mock 객체에 대해 명세를 시작하고 이는 다음 테스트의 시작점이 된다.
- 단계적으로 한번에 하나의 레이어를 대상으로 진행한다.

### 관계를 가지고 있는 객체를 순서대로 TDD

- 테스트 하고자 하는 대상이 관계를 맺고 있는 객체가 있으면 Mock 객체로 대체한 후 테스트
- 관계를 맺어야 하는 클래스가 정의되지 않았다면 새로운 클래스를 만들기

## References

- https://joont92.github.io/tdd/상태검증과-행위검증-stub과-mock-차이/
- https://martinfowler.com/articles/mocksArentStubs.html
- https://medium.com/@adrianbooth/test-driven-development-wars-detroit-vs-london-classicist-vs-mockist-9956c78ae95f
