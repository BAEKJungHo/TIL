# NEXTSTEP_2주차_STEP2_느낀점

- [Source and Code Review](https://github.com/next-step/atdd-subway-path/pull/200)

## STEP2 - 구간 제거 기능 변경

### 요구사항

- 기존에는 마지막 역 삭제만 가능했는데 위치에 상관 없이 삭제가 가능하도록 수정
- 종점이 제거될 경우 다음으로 오던 역이 종점이 됨
- 중간역이 제거될 경우 재배치를 함
- 노선에 A - B - C 역이 연결되어 있을 때 B역을 제거할 경우 A - C로 재배치 됨
- 거리는 두 구간의 거리의 합으로 정함

### 느낀점

- 할만하다
