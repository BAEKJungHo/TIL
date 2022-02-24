# NEXTSTEP_4주차_STEP1_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-fare/pull/126)

## STEP2 - 요금 조회

### 요구사항

- 경로 조회 결과에 요금 정보를 포함하세요.
- 인수 테스트 (수정) -> 문서화 -> 기능 구현 순으로 진행하세요.
- 개발 흐름을 파악할 수 있도록 커밋을 작은 단위로 나누어 구현해보세요..

## 인수 조건

```
Feature: 지하철 경로 검색

  Scenario: 두 역의 최단 거리 경로를 조회
    Given 지하철역이 등록되어있음
    And 지하철 노선이 등록되어있음
    And 지하철 노선에 지하철역이 등록되어있음
    When 출발역에서 도착역까지의 최단 거리 경로 조회를 요청
    Then 최단 거리 경로를 응답
    And 총 거리와 소요 시간을 함께 응답함
    And 지하철 이용 요금도 함께 응답함
```

## 요금 계산 방법

- 기본운임(10㎞ 이내) : 기본운임 1,250원
- 이용 거리초과 시 추가운임 부과
  - 10km초과∼50km까지(5km마다 100원)
  - 50km초과 시 (8km마다 100원)

```
9km = 1250원
12km = 10km + 2km = 1350원
16km = 10km + 6km = 1450원
```

## 느낀점

- 문서화를 조금 더 열심히 해야 겠다.
- 객체 생성 기준을 조금 더 명확하게 정해서 생성해야겠다.

## 상태패턴 응용 피드백

음 제가 말씀드린 FareHandlerContainer를 좀 더 자세히 설명드려야겠네요! 해당 컨테이너는 스프링의 IOC컨테이너나 컨버터를 맡는 컨버전서비스, 혹은 ArgumentResolver와 같은 구조를 가집니다.

```java
public interface FareHandler {
    public boolean supports(int distance);

    public int cost(int distance); // 매개변수는 달라질 수 있습니다. 
}
```

위와같은 인터페이스가 있고, 각각의 구현체는 각각이 맡을 상태를 supports를 통해 담당합니다. 이렇게요.

```java
public class NormalFareHandler implements FareHandler {
    public static final int DELIMITER = 10;

    @Override
    public boolean supports(int distance) {
        return distance <= DELIMITER;
    }

   ....
}
```

그럼 이제 Config Layer에서는 위와같은 핸들러를 HandlerContainer라는 컨테이너에 맵으로 넣든 리스트로 넣든 일급컬렉션으로 관리하든 등록해주고, 등록된 핸들러를 차례대로 돌면서 supports가 true인 경우 핸들러를 반환하여 이를 사용해 cost를 반환하도록 하는 것이죠. 여기까지는 그냥 컨테이너 활용 혹은 책임사슬패턴 정도로 사용될 수 있는데, 각각의 구현체에게 컨테이너에 대한 의존성을 주입해준다면 상태패턴 역시 가능하겠죠 😄
정호님이 말씀하신 로직을 담당하는 클래스가 제가 말한 FareHandlerContainer가 될 것 같네요!
