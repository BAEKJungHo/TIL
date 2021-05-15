# H2

- [Last Stable 버전으로 설치](https://www.h2database.com/html/download.html)
- H2 console 실행
- 처음에는 파일로 한번 생성을 해야 함
  - ex) JDBC URL : `jdbc:h2:~/querydsl`
  - 자신이 지정한 홈 디렉터리 아래에 `파일명.mv`파일 생성되었는지 확인
- 연결 끊고 다시 접속
  - ex) JDBC URL : `jdbc:h2:tcp://localhost/~/querydsl`

- yml 설정

```yml
spring:
  datasource:
  url: jdbc:h2:tcp://localhost/~/querydsl
  username: sa
  password:
  driver-class-name: org.h2.Driver
  jpa:
    hibernate:
    ddl-auto: create
    properties:
  hibernate:
  # show_sql: true
  format_sql: true
logging.level:
  org.hibernate.SQL: debug
```
