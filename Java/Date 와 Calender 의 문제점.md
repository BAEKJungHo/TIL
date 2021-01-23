# Date 와 Calender 클래스의 문제점

> https://baekjungho.github.io/java-localdate/

- 불변 객체가 아니다
  - 따라서 멀티스레드 환경에서 언제든 문제가 발생할 수 있다.
  
- Calender 는 월(Month) 값 설계가 잘못되었다.
  - 10월을 나타내는 Calender.OCTOBER 의 값은 9 이다.

이러한 문제들을 JodaTime 이라는 오픈소스를 사용하여 문제를 피했었고, Java 8 에서는 LocalDate 를 통해 해결했다.

> 자바 8 일 경우 Date 대신 LocalDate 와 LocalDateTime 을 무조건 써야한다.
 
