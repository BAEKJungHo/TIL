# 스프링 MVC 패턴

- M(Model) : 모델
  - 평번한 자바 객체 POJO
  - 도메인 객체 또는 DTO 로 화면에 전달 할 또는 전달 받은 데이터를 담고 있는 객체
- V(view) : 뷰
  - HTML, JSP, 타임리프
- C(Controller) : 컨트롤러
  - 스프링 @MVC
  - 사용자 입력을 받아 모델 객체의 데이터를 변경하거나, 모델 객체를 뷰에 전달하는 역할
  - 입력값 검증
  - 입력 받은 데이터로 모델 객체 변경
  - 변경된 모델 객체를 뷰에 전달
  
## 장점

- 동시 다발적(Simultaneous) 개발 
  - 백엔드 개발자와 프론트엔드 개발자가 독립적으로 개발을 진행할 수 있다.
- 높은 응집도
  - 논리적으로 관련있는 기능을 하나의 컨트롤러로 묶거나, 특정 모델과 관련있는 뷰를 그룹화 할 수 있다.
- 개발 용이성

## 단점

- 코드 네이게이션 복잡
- 코드 일관성 유지에 노력이 필요함
- 높은 러닝 
  
## Example

```java
@Service
public class EventService {
  public List<Event> getEvents() {
    Event event1 = Event.builder()
      .name("스프링 MVC 1")
      .limitOfEnrollment(5)
      .startDateTime(LocalDateTime.of(2019, 1, 10, 10, 0)
      .endDateTime(LocalDateTime.of(2019, 1, 10, 12, 0)
      .build();
  
    Event event2 = Event.builder()
      .name("스프링 MVC 2")
      .limitOfEnrollment(5)
      .startDateTime(LocalDateTime.of(2019, 1, 10, 10, 0)
      .endDateTime(LocalDateTime.of(2019, 1, 10, 12, 0)
      .build();
  
    return List.of(event1, event2);
  }
}
```
