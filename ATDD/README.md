# ATDD 

## 인수 테스트

- __인수 테스트__
  - 사용자의 관점에서 올바르게 작동하는지 테스트
  - 인수 조건은 기술(개발) 용어가 사용되지 않고 일반 사용자들이 이해할 수 있는 단어를 사용
  - 클라이언트가 의뢰했던 소프트웨어를 인수 받을 때, 미리 전달했던 요구사항이 충족되었는지를 확인하는 테스트
  - 인수 테스트는 소프트웨어가 기능적으로 어떤일을 해야 하는지 알려주고 확실하고 믿을 만한 원천 
  - 사용자 인수 테스트, 애자일에서의 인수 테스트 등
  - __자동화 된 테스트__
    - 인수 테스트는 수동으로도 가능하지만, 자동화되면 새로운 시스템 변경 사항이 이미 구현된 요구사항에 영향을 미치지 않는지를 확인하는 `회귀 테스트`로도 사용할 수있다.
    - NEXTSTEP 1주차 듣고 든 생각 -> 회귀 테스트가 가능한 이유가 뭘까?
      - 보통 UserStory -> Scenario 를 만들고 테스트를 함.
      - __중요한 것은 Test Code 가 최대한 Production Code 와 결합하지 않도록 하는 것이 중요.__
  - __시나리오 기반 테스트__
    - 도메인이나 기술에 대한 배경 지식이 없어도 이해할 수 있음
    - 요구사항을 명확하게 이해할 수 있음
  - __Black Box Test__
    - 인수 테스트는 Black Box 테스트 형식이다.
    - `Black Box Test` : 세부 구현에 영향을 받지 않게 구현하기 
    
> [Acceptance Test Driven Development](https://mysoftwarequality.wordpress.com/2013/11/12/when-something-works-share-it/)  

## ATDD 란 ?

- 원래는 다양한 관점을 가진 팀원(기획, 개발, 테스트 등)들과 협업을 위한 애자일 방법 중 하나
- 다른 관점에서 원활한 커뮤니케이션 없이 논의를 한다면 서로 다른 결과물을 상상하여 작업을 진행할 수 있음
- 따라서 프로덕트 결과물이 나오는 시점에서야 이해하고 있던 내용이 다름을 인지하게 되는 경우 발생
- ATDD는 이러한 리스크를 사전에 방지하고자 기획 단계부터 인수 테스트를 통해 공통의 이해를 도모하여 프로젝트를 진행
    
## TDD vs ATDD

- TDD는 개발자 영역을 다루며 시스템을 구성하는 유닛이나 모듈을 테스트 함
- TDD 설계 이슈는 특정 모듈이나 클래스가 인수 테스트 전체 또는 일부분을 통과할 수 있도록 책임을 할당
- TDD 와 ATDD 는 동일한 품질 목표를 갖고, 서로 상호 관계에 있다.
- 궁극적으로 TDD를 통해 특정 모듈이나 클래스를 담당하고 이 들이 모여 인수 테스트 전체 또는 일부분을 통과할 수 있도록 책임을 할당

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

1. + 버튼 클릭 후 Live Template 선택
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

