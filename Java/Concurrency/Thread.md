# Thread

Thread 를 배우기 전에, Process 와 Thread 의 개념에 대해서 아래의 링크를 통해서 먼저 공부하고 오면 좋다. 왜냐하면 이번 글에서는 프로세스, 쓰레드, 멀티 프로세스, 멀티 쓰레드에 대한 개념을 숙지했다고 가정하고 설명할 것이기 때문이다.

[OS : Process and Thread](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md)

## Thread 란?

Thread 의 사전적 의미는 `한 가닥의 실`이라는 의미인데, 한 가지 작업을 실행하기 위해 순차적으로 실행할 코드를 실처럼 이어 놓았다고 해서 유래된 이름이다.
자바에서는 메인 스레드가 main() 메서드를 실행하면서 시작되는데, 멀티 스레드의 경우 메인 스레드가 종료 되더라도, 실행 중인 작업 스레드가 하나라도 있으면 애플리케이션이 종료되지 않는다.

## Thread 생성

- Runnable 을 매개변수로 갖는 생성자를 호출하여 Thread 를 생성할 수 있다.

```java
Thread thread = new Thread(Runnable target);
```

Runnable 은 작업 스레드가 실행할 수 있는 코드를 가지고 있는 객체라고 해서 붙여진 이름이다.

```java
class Task implements Runnable {
    public void run() {
        // do Something
    }
}
```

위 코드를 가독성 좋게 람다식을 사용하여 표현할 수 있다.

```java
Thread thread = new Thread(() -> {
    // do Something
});
thread.start();
```

다음과 같은 방식으로도 스레드를 생성할 수 있다.

```java
Thread thread = new Thread() {
    public void run() {
         // do Something
    }
};
```

## 스레드 이름 설정하기

따로 설정을 하지 않으면 `Thread-n` (n 은 숫자) 이라는 이름으로 설정된다.

```java
thread.setName("스레드 이름");
```

## 현재 스레드 확인하기

```java
Thread.currentThread();
```

## 동시성과 병렬성

멀티 스레드 환경에서는 동시성(Concurrency) 또는 병렬성(Parallelism) 으로 실행된다.

### Parallelism

When two threads are running in parallel, __they are both running at the same time.__ For example, if we have two threads, A and B, then their parallel execution would look like this:

```java
CPU 1: A ------------------------->

CPU 2: B ------------------------->
```

### Concurrency

When two threads are running concurrently, their execution overlaps. Overlapping can happen in one of two ways: either the threads are executing at the same time (i.e. in parallel, as above), or their executions are being interleaved on the processor, like so:

```java
CPU 1: A -----------> B ----------> A -----------> B ---------->
```

동시성은 스레드 스케줄링에 의해서 어떤 순서로 동시성이 실행될 것인지 정해지는데, 스레드 스케줄링에 의해 스레드들은 아주 짧은 시간에 번갈아가면서 그들의 작업(run())을 조금씩 실행한다.

## 자바의 스레드 스케줄링

자바의 스레드 스케줄링은 `우선순위(Priority)` 방식과 `순환 할당(Round-robin)` 방식을 사용한다.

> 숫자가 높을 수록 우선순위가 높다. 1이 가장 낮고, 10이 가장 높다. 우선순위를 부여하지 않으면 모든 스레드들은 기본적으로 5의 우선순위를 할당 받는다.

우선순위 방식은 스레드 객체에 우선순위 번호를 부여할 수 있다. 하지만 순환 할당 방식은 JVM 에 의해서 정해지기 때문에 코드로 제어할 수 없다.

```java
thread.setPriority(Thread.MAX_PRIORITY);
thread.setPriority(Thread.NORM_PRIORITY);
thread.setPriority(Thread.MIN_PRIORITY);
```

## 공유 객체를 사용할 때의 주의점

공유 객체를 사용할 때에 동시성 이슈가 발생할 수 있다.

[동시성 이슈](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md#%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%9D%B4%EC%8A%88) 해당 글에 자세히 설명 되어있다.

## ThreadLocal

[ThreadLocal](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md#threadlocal) 글을 참고 하도록 하자.

## References

- [이것이 자바다](http://www.yes24.com/Product/Goods/15651484)
- https://www.eginnovations.com/blog/java-threads/
- https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism
- https://stackoverflow.com/questions/34689709/java-threads-and-number-of-cores/34689857#34689857
