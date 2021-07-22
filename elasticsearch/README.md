# Elasticsearch

## [Java High Level Rest Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high.html)

엘라스틱 서치가 6.3.0 으로 올라가면서 내부 client 로 SQL 문법을 지원하게 되었다.

Elasticsearch Docs 에는 Java High Level Rest Client 에 대해 다음과 같이 설명 되어있다.

__The Java High Level REST Client works on top of the Java Low Level REST client.__ 즉, Java 상위 수준 REST 클라이언트는 Java 하위 수준 REST 클라이언트 위에서 작동한다.

### 호환성(Compatibility)

Java High Level Rest Client 의 자바 최소 사양은 1.8 이다. 클라이언트 버전은 클라이언트가 개발된 Elasticsearch 버전과 동일하다.

### [Java docs](https://artifacts.elastic.co/javadoc/org/elasticsearch/client/elasticsearch-rest-high-level-client/7.13.4/index.html)

### Maven Repository

The high-level Java REST client is hosted on Maven Central. The minimum Java version required is 1.8.

- maven

```java
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.4.2</version>
</dependency>
```

- gradle

```java
dependencies {
    compile 'org.elasticsearch.client:elasticsearch-rest-high-level-client:7.4.2'
}
```

### 종속성(Dependencies)

- org.elasticsearch.client:elasticsearch-rest-client
- org.elasticsearch:elasticsearch

### 초기화(Initialization)

RestHighLevelClient 인스턴스는  REST low-level client builder 를 다음과 같이 구축 할 수있다.

```java
RestHighLevelClient client = new RestHighLevelClient(
        RestClient.builder(
                new HttpHost("localhost", 9200, "http"),
                new HttpHost("localhost", 9201, "http")));
```

상위 수준 클라이언트는 제공된 빌더를 기반으로 요청을 수행하는 데 사용되는 하위 수준 클라이언트를 내부적으로 생성합니다. 
저수준 클라이언트는 연결 풀을 유지 관리하고 일부 스레드를 시작하므로 제대로 완료되면 상위 수준 클라이언트를 닫아야 하며, 그러면 내부 저수준 클라이언트를 닫아 해당 리소스를 해제합니다. 
이것은 다음을 통해 수행할 수 있습니다 close.

```java
client.close();
```

### [Java Low Level Rest client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-low-usage-requests.html#java-rest-low-usage-request-options)

### [검색 API](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-search.html)

- SearchRequest : 검색 요청

```java
SearchRequest searchRequest = new SearchRequest();
searchRequest.indices(indexName);
searchRequest.source(searchSourceBuilder);
```

- SearchResponse : 검색 응답

```java
// client is RestHighLevelClient
searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
```

#### [QueryBuilder](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-query-builders.html)

쿼리 작성을 위해 제공되는 Builder 이다.
