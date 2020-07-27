# @Builder

- Builder 패턴을 사용하는 이유
  - Type-safety (형식 안정성)
  - 불변 객체로 사용하기 위해
- 단점
  - 데이터 양이 많은 경우 DB 에서 직접 조회해서 VO 담아서, 다시 DTO 로 변경하려면 속도 저하가 심하다. (직접 DB 에서 조회해서 바인딩하는게 훨씬 빠름)
  - MyBatis 의 collection 이나 association 의 경우 속도 저하가 심하다. (오히려 따로 두 번 조회해서 처리하는게 빠르기도 함)
  
 
