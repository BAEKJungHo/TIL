# ShedLock

같은 잡을 수행하는 각각 다른 서버에서의 인스턴스 A,B가 있을 때, A,B 둘 중 하나가 수행하도록, 2개 이상의 서버에서 중복 수행을 방지하도록 Lock 을 걸게 하는 라이브러리로 shedlock 이 있다.

순서 관계없이 lock을 설정한 시간동안은 하나의 인스턴스만 작동하도록 되어있다.

```java
// 최소 5분동안 잠금을 유지(lockAtLeastForString) = ShedLock에서 이 메서드를 5분마다 실행할 수 있다는 의미
// 실행 노드가 죽을 경우 잠금을 유지해야 하는 기간 설정(lockAtMostForString): 14분 이상은 잠기지 않음 
@SchedulerLock(name = "TaskScheduler_scheduledTask", lockAtLeastForString = "PT5M", lockAtMostForString = "PT14M")
```

- 1초마다 수행하는 스케줄러

```java
@Scheduled(cron = "* * * * * *", zone = "Asia/Seoul")
@SchedulerLock(name = "ClassName_methodName", lockAtMostFor = "1s", lockAtLeastFor = "1s")
```

## Links

- https://www.baeldung.com/shedlock-spring
