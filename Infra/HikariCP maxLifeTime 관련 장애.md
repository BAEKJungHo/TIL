# HikariCP maxLifeTime 관련 장애

넥스트스텝에서 장애가 발생한 적이 있었다.

![nextstep-1](https://user-images.githubusercontent.com/47518272/178135792-cd324ef3-a314-4c3a-800c-91931f44d16d.png)

정답은 WAS 의 응답이 제대로 나오지 않는다는 이슈이다.

![nextstep-2](https://user-images.githubusercontent.com/47518272/178135825-0add7bba-d6bd-459f-9c8b-704190d9e731.png)

![nextstep-3](https://user-images.githubusercontent.com/47518272/178135855-29bf81a4-e29a-43a2-8a93-0867bd5b95be.png)

![nextstep-4](https://user-images.githubusercontent.com/47518272/178135879-1bb184e4-4267-4cf0-b760-00b24d30d9e5.png)

`Possibly consider using a shorter maxLifetime value`

- MySQL 의 wait_timeout 설정을 HikariCP 의 max-lifetime 설정보다 높게 설정하라는 의미
- wait_timeout 은 활동하지 않는 커넥션을 끓을 때 까지 서버가 대기하는 시간
  - sleep 상태 커넥션을 끊는 주기
- max-lifetime 은 HikariCP 가 강제로 Connection 을 종료하기까지의 시간

![nextstep-5](https://user-images.githubusercontent.com/47518272/178136012-ce245269-8952-4f7a-bbac-efc70a55ece6.png)

따라서, MySQL 설정으로 HikariCP 가 종료하기 전에 연결이 빠르게 끊길 경우 에러가 발생한다.

![nextstep-6](https://user-images.githubusercontent.com/47518272/178136049-885ef788-8b24-42d7-8917-4170d2f73d34.png)

![nextstep-7](https://user-images.githubusercontent.com/47518272/178136152-3ed43f21-a957-40bc-8643-63cc9511e691.png)

![nextstep-8](https://user-images.githubusercontent.com/47518272/178136163-6a542e33-4c60-42fd-b950-1e91528e1d1a.png)

![nextstep-10](https://user-images.githubusercontent.com/47518272/178136389-ffed1646-67e5-4747-a052-9d334b42cd19.png)
