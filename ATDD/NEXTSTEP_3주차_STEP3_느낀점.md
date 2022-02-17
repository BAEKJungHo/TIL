# NEXTSTEP_3주차_STEP3_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-favorite/pull/208)

## STEP2 - 즐겨찾기 기능 구현

### 요구사항

- 즐겨 찾기 기능을 구현하기
- 로그인이 필요한 API 요청 시 유효하지 않은 경우 401 응답 내려주기

## 인수 조건

```
Feature: 즐겨찾기를 관리한다.

  Background 
    Given 지하철역 등록되어 있음
    And 지하철 노선 등록되어 있음
    And 지하철 노선에 지하철역 등록되어 있음
    And 회원 등록되어 있음
    And 로그인 되어있음

  Scenario: 즐겨찾기를 관리
    When 즐겨찾기 생성을 요청
    Then 즐겨찾기 생성됨
    When 즐겨찾기 목록 조회 요청
    Then 즐겨찾기 목록 조회됨
    When 즐겨찾기 삭제 요청
    Then 즐겨찾기 삭제됨
```
