# NEXTSTEP_1주차_STEP1_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-map/pull/198)

## STEP1 - 노선 관리 기능 구현

### 인수 조건

```java
Feature: 지하철 노선 관리 기능

  Scenario: 지하철 노선 생성
    When 지하철 노선 생성을 요청 하면
    Then 지하철 노선 생성이 성공한다.
  
  Scenario: 지하철 노선 목록 조회
    Given 지하철 노선 생성을 요청 하고
    Given 새로운 지하철 노선 생성을 요청 하고
    When 지하철 노선 목록 조회를 요청 하면
    Then 두 노선이 포함된 지하철 노선 목록을 응답받는다
    
    ...
```

### 느낀점

- STEP1 이라, 아직은 쉬운 것 같다.
- 확실히 70만원의 값어치를 하는 것 같다.
- SI 에서 일할 땐, REST API 설계에 크게 신경을 쓰지 않았고, 그래도 문제가 되지 않았는데 이번 기회를 통해서 REST API 설계에도 많은 신경을 써야 겠다라는 것을 느꼈다.
