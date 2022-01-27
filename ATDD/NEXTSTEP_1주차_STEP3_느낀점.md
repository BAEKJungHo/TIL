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

### Q. SectionService 는 분리하는게 맞는가?

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

### A. 답변

저 같은 경우는 유스케이스를 기준으로 분리를 하게되는 편입니다. 현재는 간단한 미션이지만 실무를 진행하면서 요구사항이 복잡해질수록 처음에는 UserService로 시작했다가, UserQueryService, UserSignUpService, UserUpdateService 등과 같이 나누어 지게 되더라구요............
정호님은 사용하셨을 때 어떤 방향이 가장 좋은 것 처럼 느껴지셨는지도 궁금하네요 👀

### A. 나의 답변

저는 주로 `Persistence Layer 에 대한 의존성을 고려하여 분리` 하였던거 같은데,,
저 기준을 사용한 제 나름대로의 이유는 아래와 같았습니다.

- 이미 Controller 의 핸들러메서드를 통해 자원과 행위가 표현되어 있기 때문에
`lines/{lilneId}/sections/{sectionId}` 를 보고 두 관계를 유추할 수 있다고 봅니다.

- LineService 에서 Section 을 관리하게되면 자연스럽게 StationRepository 의존성이
생기고, 지금은 섹션을 등록하기 위해서 StationService 에서 재사용해야하는 메서드가 `findStationById` 하나지만,, 이게 재사용해야하는 메서드가 많으면 많을 수록 누군가는 `Repository` 가 아닌 `Service` 에 의존하게끔 설계할 수도 있다고 생각합니다.

- 그렇게 되면 설계에 따라서 `순환참조` 발생가능성이 더 커진다고 봐서 저는 Persistence Layer 에 대한 의존성을 고려해서 분리했던것 같습니다.

- 만약에 LinService 에 명령과 쿼리로 분리되어 FindService, CommandService 이런식으로 나누어진다고 했을때, 과연 saveSection 은 어느 Service 에 들어가야 하는가를 생각해봤을때

- CommandService 에 들어가는 순간 FindService 에 있는 조회 메서드를 재사용할 수 없어서 메서드를 새로 만들어서 사용해야 하지 않을까 합니다. 또한 saveSection 에 대한 검증을 진행하기 위해서 CommandService 는 자연스럽게 StationRepository 의존성을 받게되므로, 누군가 __LineCommandService 를 보았을때, 노선을 등록하기 위해서 역에 의존하고 있다라고 보여질 수도 있다고 봅니다.__

- 이게 지금은 애플리케이션 서비스하나만 사용하고 있긴하지만, 도메인으로부터 파생된 로직을 `도메인 서비스`로 만들어서 관리하다고 했을때 아래와 같이 되지않을까 합니다.

```java
public class LineQueryService {
    private final LineRepository lineRepository;
    private final LineService lineService; // 도메인 서비스
    private final SectionService sectionService; // 도메인 서비스
    private final StationRepository stationRepository; // 구간 조회 시 필요

    // LineQueryService 에 현재 도메인서비스가 존재하는 이유는, 노선과 구간을 각각 조회할때 필요한 도메인 로직이 있다고 가정

    // 생략
}
```

- LineQueryService 에 벌써 많은 의존성이 생기고, 실제로 이렇게 되면 __필드 부분(어떤 의존성을 주입 받고 있는지)을 보더라도 라인과 구간의 관계를 명확하게 파악하기 힘들다고 보여집니다.__

> 라인과 구간의 관계는 REST API 설계와 엔티티간의 관계를 통해서 파악하는게 훨씬 빨라지게 된다고 생각합니다.

### Q. 커스텀 익셉션은 과연 언제만드는것이 좋은가?

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

### A. 답변

커스텀 익셉션 같은 경우, 팀내 컨벤션을 따르는게 가장 좋을 것 같고, 저는 굳이 따지자면 구체화된 커스텀 익셉션을 선호하는 편 입니다.
구체화된 익셉션이 로그에서 찾거나 예외처리를 한다거나 할 때 개인적으로는 조금 더 편했던 것 같아요 :)

다양한 의견들이 있으니 한번 찾아보고 선호하시는 방향을 정해서 사용해보시면 좋을 것 같네요 👍

https://tecoble.techcourse.co.kr/post/2020-08-17-custom-exception/
