# Hikaricp 

## spring boot with hikaricp

> Spring Boot 2.x 버전 부터 Hikaricp 를 기본 DBCP 로 사용한다.
>
> https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#data.sql.datasource.connection-pool

## max-lifetime 의 알맞은 설정 값 찾는 방법

- __MySQL 의 timeout 관련 옵션__
  - interactive_timeout
    - `mysql>` 과 같은 콘솔이나 터미널 모드에서 mysqld 와 client 가 연결을 맺은 다음 요청을 기다리는 최대시간
  - wait_timeout
    - API 를 이용한 client 프로그램(PHP, JDBC, ODBC...) 상에서 최대 연결시간을 말하며, 설정된 시간 동안 아무 요청이 없으면 연결은 취소되고 다시 요청이 들어오면 자동으로 연결이 맺어짐
- __HikariCP 의 max-lifetime 이란__
  - Connection 의 최대 유지 시간을 의미
  - 커넥션을 생성한 이후, 이 시간이 지나면 커넥션을 삭제, 삭제한 뒤 커넥션을 새로 생성
  - Pool 의 Connection 의 최대수명을 제어. 현재 사용중인 connection 은 폐기되지 않으며, connection 이 closed 상태일 때만 제거


## Links

- [About Pool Sizing](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing)
- https://may9noy.tistory.com/26
- https://hello-world.kr/33#id-[Spring]HikariPoolPossiblyconsiderusingashortermaxLifetimevalue-3.HikariCP%EC%9D%98%EC%A3%BC%EC%9A%94%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0
