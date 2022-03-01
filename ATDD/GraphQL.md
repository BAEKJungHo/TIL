# REST와 GraphQL

## API 를 잘 설계 해야하는 이유

- 모든 부분을 설명하지 않고 새로운 부분에 대한 설명만 하면 됨
- 불필요한 커뮤니케이션 비용을 줄임

## 표준

- 여러 해를 거쳐 가며 HTTP 표준을 정하고 있음
- 표준은 RFC 를 통해 확인할 수 있음
- HTTP 기반으로 API 를 설계한다면 해당 문서를 고려하는 게 좋음

## REST는 표준인가?

- 아니다.
- 효과적으로 네트워크 아키텍처를 설계하는데 도움을 주는 스타일
- 즉, 몇가지 제약조건과 지향하는 바는 있지만 정해진 답이 없다.

## 궁긍적으로는

- 애플리케이션 API 의 요구사항을 바탕으로
- 아키텍처 스타일이 가지는 특징을 참고하여
- 공통의 약속(API 설계)을 정해야 함

## GraphQL

- API 를 위한 쿼리 언어
- 클라이언트에게 필요한 것을 정확하게 제공

```
{
  hero {
    name
    height
    mass
  }
}
```
```
{
  "hero": {
    "name": "Luke Skywalker",
    "height": 1.72,
    "mass": 77
  }
}
```

### Why GraphQL

- 기능이 점점 복잡해지면서 API의 Versioning에 대한 니즈가 발생
- RESTful 방식으로 클라이언트 별로 데이터를 준비하기에는 상당한 리소스가 많이 발생

### GraphQL 특징

- 주로 하나의 Endpoint 를 사용한다.
- 요청할 때 사용한 Query 문에 따라 응답의 구조가 달라진다.

## GraphQL in Spring Boot

### build.gradle

```
// graphql
implementation 'com.graphql-java:graphql-spring-boot-starter:5.0.2'
implementation 'com.graphql-java:graphql-java-tools:5.2.4'
```

### resources/sample.graphqls

```
type Station {
    id: ID!
    name: String!
}

# The Root Query for the application
type Query {
    stations: [Station]!
}
```

### StationQuery

```java
@Component
public class StationQuery implements GraphQLQueryResolver {
    private final StationRepository stationRepository;

    public StationQuery(StationRepository stationRepository) {
        this.stationRepository = stationRepository;
    }

    public List<StationResponse> getStations() {
        final List<Station> stations = stationRepository.findAll();
        return StationResponse.listOf(stations);
    }
}
```

### StationQueryTest.java

```java
@DisplayName("지하철역을 조회한다.")
@Test
void getStations() throws JsonProcessingException {
    지하철역_등록되어_있음("강남역");

    String query =
            "{ \n" +
            "    \"query\" : \"{" +
            "        stations {" +
            "            id" +
            "            name" +
            "        }" +
            "    }\"\n" +
            "}";

    ExtractableResponse<Response> response = RestAssured
            .given().log().all()
            .contentType(MediaType.APPLICATION_JSON_VALUE)
            .body(query)
            .when().post("/graphql")
            .then().log().all().extract();

}
```

### sample.graphqls

```
type Mutation {
    createStation(name: String!) : Station!
}
```

### StationMutation.java

```java
@Component
public class StationMutation implements GraphQLMutationResolver {
    private final StationRepository stationRepository;

    public StationMutation(StationRepository stationRepository) {
        this.stationRepository = stationRepository;
    }

    public void createStation(String name) {
        Station station = new Station(name);
        stationRepository.save(station);
    }
}
```

> https://www.baeldung.com/spring-graphql

## GraphQL 이 본 REST 의 단점

- 오버페칭
- 언더페칭
- Endpoint 관리

## 실생활에서의 GraphQl

- FaceBook API
- Github API
- 필요한 데이터만 정의해 사용할 수 있다는 강력한 장점을 지니고 있음

## GraphQL 장점

- HTTP 요청의 횟수를 줄일 수 있다.
- HTTP 응답의 Size 를 줄일 수 있다.

## GraphQL 단점

- File 전송 등 Text 만으로 하기 힘든 내용들을 처리하기 복잡하다.
- 고정된 요청과 응답만 필요할 경우에는 Query 로 인해 요청의 크기가 RESTful API 의 경우보다 더 커진다.
