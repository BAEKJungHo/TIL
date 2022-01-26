# NEXTSTEP_1주차_STEP3_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-map/pull/250)

## STEP3 - 인수 테스트 리팩터링

### 요구사항

- 지하철 노선 생성 시 필요한 인자 추가하기
- 지하철 노선에 구간을 등록하는 기능 구현
- 지하철 노선에 구간을 제거하는 기능 구현
- 지하철 노선에 등록된 구간을 통해 역 목록을 조회하는 기능 구현
- 구간 등록 / 제거 시 예외 케이스에 대한 인수 테스트 작성

### 느낀점

- 3일을 걸쳐서 진행하였다.
- JPA 에 아직 익숙하지 않다.
- 힘든 만큼 많은 것을 느낀다.

## QNA

안녕하세요 리뷰어님! 

이번 미션은 꽤 난이도가 있다고 느껴졌으며,, 1주차에서 가장 많은 시간을 투자하게된 STEP 이었던것 같습니다.

이번 미션을 진행하면서 가장 머리 아프게 했던 고민들에 대해서 질문 드리겠습니다!

### SectionService 는 분리하는게 맞는가?

구간을 만들때, REST API를 설계하는 것은 어렵지 않았습니다. 구간은 노선이 등록되고 나서야 등록 가능하기 때문에, `노선/노선ID/구간/구간ID` 형식으로 URI 를 표현하는게 맞다고 생각이 들었습니다.

문제는 Service 였는데,, 제가 한 고민은 아래와 같습니다.

1. LineService 에서 Section 의 등록, 삭제 등의 유스케이스를 나타내게 되면 기존에 없던 StationRepository 의존성이 생긴다고 판단하여 분리하였습니다.

2. 현재 서비스들이 application 패키지 밑에 존재하며, 실제로 하고 있는 역할도 도메인 서비스가 아닌 유스케이스를 구현하는 `애플리케이션 서비스`라고 생각합니다. 

애플리케이션 서비스이다보니, Presentation Layer (LineController) 에 있는 API 기능을 LineService 에 관리하도록 하는게 맞는것 같기도하고..

Line Entity 와 Section Entity 의 관게를 보면, Line Entity 에서 Section Entity 의 생명주기를 관리하고 있긴하지만,, 애플리케이션 서비스의 분리 기준이 생명주기로 하는게 맞는거가 의문이 들기도하였습니다.

- __가장 궁금한 내용 : 애플리케이션 서비스를 분리하는 기준__
    - 1. 각 애플리케이션 서비스의 Persistence Layer 에 대한 의존성을 고려하여 분리하는 지
    - 2. 유스케이스를 기준으로 분리하는 지
    - 3. 엔티티의 생명주기를 기준으로 분리하는 지

그 외에도 다양한 기준이 있을 것 같긴한데,, 

이에 대한 리뷰어님 생각은 어떠신지 궁금합니다.

### 커스텀 익셉션은 과연 언제만드는것이 좋은가?

- 커스텀 익셉션의 장점은 Class 이름만 보고 어떠한 에러인지 더 구체적으로 알 수 있고, 입맛에 맞게 커스텀할 수 있다는 것 같은데

문제는 `커스텀 익셉션 생성 기준`을 어떻게 잡아야할지 명확하게 감이 오질 않습니다.

```java
private Station findStationById(final Long id) {
    return stationRepository.findById(id)
            .orElseThrow(NotFoundStationException::new);
}
```

예를 들어 위와 같은 코드는 커스텀 익셉션 대신 RuntimeException 을 사용하여 메시지를 생성자 파라미터로 던져서 사용가능하기도 한데,

```java
if(getIds(registeredDownStations).contains(downStationId) || getIds(registeredUpStations).contains(downStationId)) {
    throw new ValidationException("새로운 구간의 하행역은 현재 등록되어있는 역일 수 없습니다.");
}
```

이런 경우를 어떤식으로 처리해야 효과적인지 잘 모르겠습니다.

일단 위 제약조건은 도메인에 파생된 로직이라 판단해서 Section 도메인에서 관리하고, 등록하기전 검증을 하는 것이기 때문에 검증 에러라고 판단하여,

기존에 BeanValidation 처리를 위해 만들었던 ValidationException 을 따로 메시지만 넘길 수 있도록 커스텀하여 사용하도록 하였는데..

맞게 한건지,, 더 나은 방법이 있는지 궁금합니다.

