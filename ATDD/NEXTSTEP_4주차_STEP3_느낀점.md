# NEXTSTEP_4주차_STEP3_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-fare/pull/146)

## STEP3 - 요금 정책 추가

### 요구사항

- __노선별 추가 요금__
  - 추가 요금이 있는 노선을 이용 할 경우 측정된 요금에 추가
    - ex) 900원 추가 요금이 있는 노선 8km 이용 시 1,250원 -> 2,150원
    - ex) 900원 추가 요금이 있는 노선 12km 이용 시 1,350원 -> 2,250원
  - 경로 중 추가요금이 있는 노선을 환승 하여 이용 할 경우 가장 높은 금액의 추가 요금만 적용
    - ex) 0원, 500원, 900원의 추가 요금이 있는 노선들을 경유하여 8km 이용 시 1,250원 -> 2,150원
- __로그인 사용자의 경우 연령별 요금으로 계산__
  - 청소년: 운임에서 350원을 공제한 금액의 20%할인
  - 어린이: 운임에서 350원을 공제한 금액의 50%할인

## 느낀점

- __@AuthenticationPricipal 로 LoginMember 를 받아올 때, 로그인하지 않은 경우를 처리하기 위한 방법을 배웠다.__
  - 첫 번째. 어노테이션에 `required = false` 옵션을 줘서 처리
  - 두 번째. ArgumentResolver 구현체에서 Authentication 이 null 이면 예외 대신 null 반환
  - 세 번째. 정책적인 요소 때문에 별도의 상수 값을 갖거나, 기능을 가져야하는 경우 `Null Object Pattern` 사용 
    - Null Object 인 NonLoingMember 를 구현할 때 LazyHolder 를 이용한 싱글턴 패턴을 사용했다.


