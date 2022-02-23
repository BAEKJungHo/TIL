# Void 타입 조롱하기

### 문서화 테스트의 경우 : Controller 단을 테스트하는 경우

```java
@Test
void 역_삭제() {
    doNothing().when(stationService).deleteStationById(anyLong());
    ExtractableResponse<Response> response = 역_삭제_요청(spec);
    역_삭제_성공(response);
}
```

### Service 단을 테스트하는 경우

```java
@Test
void deleteSection() {
    // given
    when(stationService.findStationById(1L)).thenReturn(교대역);
    when(stationService.findStationById(2L)).thenReturn(역삼역);
    when(stationService.findStationById(5L)).thenReturn(강남역);
    when(lineRepository.findById(1L)).thenReturn(Optional.of(이호선));
    lineService.addSection(교대_TO_역삼, 이호선.getId());
    lineService.addSection(강남_TO_역삼, 이호선.getId());

    // when
    lineService.deleteSection(이호선.getId(), 강남역.getId());

    // then
    Line line = lineRepository.findById(이호선.getId()).get();
    Sections sections = line.getSections();
    assertThat(sections.getSections().get(0).getDistance()).isEqualTo(7);
}
```
