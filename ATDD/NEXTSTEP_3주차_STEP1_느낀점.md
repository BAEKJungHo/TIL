# NEXTSTEP_3주차_STEP1_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-favorite/pull/158)

## STEP1 -토큰 키반 로그인 구현

### 요구사항

- 패키지 구조 리팩터링(선택)
- MemberAcceptanceTest 의 인수 테스트 통합하기
- AuthAcceptanceTest 의 myInfoWithSession 테스트 메서드를 성공 시키기
- AuthAcceptanceTest 의 myInfoWithBearerAuth 테스트 메서드를 성공 시키기
  - TokenAuthenticationInterceptor 구현하기
- MemberAcceptanceTest 의 manageMyInfo 성공 시키기
  - @AuthenticationPrincipal 을 활용하여 로그인 정보 받아오기

### 느낀점

- [현구](https://github.com/kang-hyungu)님의 피드백이 좋았고 `anyMatch()` 랑 `@Spy` 기능에 대해 알게 되었다.
- 메서드명을 너무 길게 작성하지 않도록 노력해봐야겠다.
- 몇가지 의견이 다른 부분은 있었다.

## 피드백

### Q 

라인 정적 팩터리 메서드한테 다시 같은 타입의 객체인 request.toEntity()를 전달하는 것보다 객체 생성에 필요한 값만 전달하면 되지 않을까요?

```java
Suggested change 
        Line line = Line.of(request.toEntity(), upStation, downStation, request.getDistance());
        Line line = request.toEntity(upStation, downStation, request.getDistance());
```

#### A

음 Line.of 내부에 첫 번째 구간을 등록하는 아래와 같은 코드가 존재합니다.

newLine.sections.addFirst(Section.of(newLine, upStation, downStation, distance));
만약에, 현구님이 피드백 주신 것처럼 리팩토링을 하게되면, Line 엔티티 생성 규칙(노선과 첫 구간 등록)을 DTO 에서 담당하기 때문에

__엔티티 생성 규칙이 DTO 에 의존 한다고 생각이 드는데요__

현구님 리뷰를 참고해서, Line.of 에 같은 타입의 객체를 넘기는 것 말고, 아래와 같이 리팩토링 하는 것에 대해서는 어떻게 생각하시나요?

```java
Line.of(request.getName(), request.getColor(), upStation, downStation, request.getDistance());
```

### Q

dto는 단순 값만 옮기는 클래스입니다.
삼항연산자나 if, 비교문 같은 로직 처리가 있으면 안 됩니다.
이 메서드를 포함해서 isRequiredReOrdering, createReOrderedSections, reOrderSections, createStations 메서드를 적절한 도메인 클래스로 옮겨보시기 바랍니다.

#### A

> dto는 단순 값만 옮기는 클래스입니다.

말씀하신 dto 에 대한 정의는 동의합니다. 제가 저 코드를 DTO 에 넣은 이유는, 노선을 조회할 때,  StationResponse 목록을  응답에 담에서 반환하는데요,, 

__역 응답 정보를 생성하는 로직은 엔티티보다 DTO 에 정의하는 것이 더 역할과 책임을 잘 구분한게 아닌가 생각이 듭니다.__

도메인 클래스로 역에 대한 응답 정보를 생성하는 로직을 넣게되면, 응답 정보 생성 로직이 바뀔 때마다, 도메인이 수정되어야 한다고 생각이 드는데

`isRequiredReOrdering, createReOrderedSections, reOrderSections, createStations` 해당 메서드들이 응답 생성 로직 뿐만 아니라 다른 곳에서도 사용이 된다고 하면, 그때 리팩토링을 고민해도 될 것 같다고 저는 생각합니다.

현구님 생각은 어떠신가요?

#### Re: A

제가 보기엔 역 응답 정보를 생성하는게 아니라 정렬하는 로직으로 보이네요.
응답 정보를 생성한다는 범위를 넓게 잡으셔서 dto에 추가하신 것 같습니다.
지하철 역 순서를 정하는 것은 뷰의 책임이 아닌 도메인이 책임을 가져가는 게 맞습니다. 정렬 로직이 바뀌면 당연히 도메인 객체도 수정해야하구요.
프로그램을 http가 아닌 콘솔이나 모바일로 확장한다고 했을 때 dto에 정렬 로직을 두면 http, 콘솔, 모바일 세 곳에 중복 코드가 발생합니다.
