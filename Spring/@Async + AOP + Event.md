# @Async + AOP + Event

https://supawer0728.github.io/2018/03/24/spring-event/

## @Async + AOP

- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/task/SimpleAsyncTaskExecutor.html
- https://www.baeldung.com/thread-pool-java-and-guava
- https://github.com/NKLCWDT/cs/blob/main/Spring/%40Scheduled%2C%20%40Async%2C%20%40SessionAttribute%20vs%20%40SessionAttributes.md
- [SimpleAsyncTaskExecutor 를 사용하면 안되는 이유](https://heowc.tistory.com/68)
- https://www.baeldung.com/java-threadpooltaskexecutor-core-vs-max-poolsize

## ThreadPoolTaskExecutor

```kotlin
@Bean("beanName")
fun orderAcceptThreadPoolTaskExecutor(): Executor {
    val executor = ThreadPoolTaskExecutor().apply {
        corePoolSize = 5
        maxPoolSize = 30
        setQueueCapacity(80)
        setThreadNamePrefix("My-Async")
        initialize()
    }
    return executor
}
```

- corePoolSize
  - 동시에 실행 시킬 스레드의 수
- maxPoolSize
  - 스레드 풀의 최대 사이즈
  - maxPoolSize 는 ThreadPoolTaskExecutor 가 대기열의 항목 수가 queueCapacity 를 초과하는 경우에만 새 스레드를 생성 한다는 점 에서 queueCapacity 에 의존한다.
- setQueueCapacity
  - 스레드 풀의 큐 사이즈
  - corePoolSize 를 넘어서는 요청이 들어왔을 때, queue 에 task 가 쌓이게 되고, 최대로 maxPoolSize 만큼 쌓일 수 있다.


> [corePoolSize vs maxPoolSize](https://www.baeldung.com/java-threadpooltaskexecutor-core-vs-max-poolsize)
>
> [테스트 소스](https://github.com/eugenp/tutorials/blob/master/spring-threads/src/test/java/com/baeldung/threading/ThreadPoolTaskExecutorUnitTest.java)
