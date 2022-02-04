# NEXTSTEP_2주차_STEP1_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-path/pull/186)

## STEP1 - 구간 추가 기능 변경

### 요구사항

- 기존 구간의 역을 기준으로 새로운 구간을 추가
  - 기존 구간 A-C에 신규 구간 A-B를 추가하는 경우 A역을 기준으로 추가
  - 기존 구간과 신규 구간이 모두 같을 순 없음(아래 예외사항에 기재됨)
  - 결과로 A-B, B-C 구간이 생김
- 새로운 길이를 뺀 나머지를 새롭게 추가된 역과의 길이로 설정

### 느낀점

- 어렵다
