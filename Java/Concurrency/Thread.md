# Thread

Thread 를 배우기 전에, Process 와 Thread 의 개념에 대해서 아래의 링크를 통해서 먼저 공부하고 오면 좋다. 왜냐하면 이번 글에서는 프로세스, 쓰레드, 멀티 프로세스, 멀티 쓰레드에 대한 개념을 숙지했다고 가정하고 설명할 것이기 때문이다.

> Thread 면접 포인트는 아래 포스팅 안에서 많이 나오지 않을까 생각한다.

- [OS : Process and Thread](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md)
    - [동시성 이슈 : 공유 객체를 사용할 때의 주의점](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md#%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%9D%B4%EC%8A%88)
    - [ThreadLocal](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md#threadlocal)

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

## Callable 

Callable 은 Runnable 의 단점을 보완하기 위해서 만들어졌다.

```java
public interface Runnable {
    public void run();
}
```

run() 메소드는 결과 값을 리턴하지 않기 때문에, run() 메소드의 실행 결과를 구하기 위해서는 공용 메모리나 파이프와 같은 것들을 사용해서 결과 값을 받아야만 했다. 이런 Runnable 인터페이스의 단점을 없애기 위해 추가된 것이 바로 Callable 인터페이스이다.

```java
public Interface Callable<V> {
    V call() throws Exception
}
```

## Future

자바 5부터 미래의 어느 시점에 결과를 얻는 모델로 `Future` 인터페이스를 제공하고 있다. 비동기 계산을 모델링하는데 주로 사용되며, 시간이 걸릴 수 있는 작업을 Future 내부로 설정하면된다.

> Ex. 단골 세탁소에 한 무더기의 옷을 드라이클리닝 서비스를 맡기는 동작에 비유할 수 있다. 세작소 주인은 드라이클리닝이 언제 끝날지 적힌 `영수증(Future)`를 줄 것이며, 드라이니클리닝이 진행되는 동안 우리들은 다른 일을 할 수 있다.

Future 를 사용하려면 시간이 오래 걸리는 작업ㅇ르 `Callable` 객체 내부로 감싼 다음에 `ExecutorService` 에 제출해야 한다.

### 자바 8 이전의 코드

```java
// ThreadPool 에 task 를 제출하려면 ExecutorService 를 만들어야 한다.
// Callable 을 ExecutorService 에 제출한다.
ExecutorService executor = Executors.newCachedThreadPool();
Future<Double> future = executor.submit((Callable<Double>) () -> {
    return doSomeLongComputation(); // 시간이 오래 걸리는 작업은 다른 스레드에서 비동기적으로 실행한다.
});

doSomethingElse(); // 비동기 작업을 수행하는 동안 다른 작업 수행

try {
    // 비동기 작업의 결과를 가져온다. 결과가 준비되어 있지 않으면 호출 스레드(doSomethingElse())가 블록된다. 
    // 하지만 최대 1초까지만 기다린다.
    Double result = future.get(1, TimeUnit.SECONDS); 
} catch (ExecutionException e) {
    // 계산 중 예외 발생
} catch (InterruptedException e) {
    // 현재 스레드에서 대기 중 인터럽트 발생
} catch (TimeoutException e) {
    // Future 가 완료되기 전에 타임아웃 발생
}
```

![executor](https://user-images.githubusercontent.com/47518272/153706901-a2e3b001-ff66-4806-94aa-4f235d2ade2b.png)

이 시나리오의 문제는 오래 걸리는 작업이 영원히 끝나지 않으면 문제가 생길 수 있다는 것이다. 따라서, get 메서드를 오버로드해서 우리 스레드가 대기할 최대 타임아웃 시간을 정하는 것이 좋다.

## References

- [이것이 자바다](http://www.yes24.com/Product/Goods/15651484)
- [Modern Java In Action](http://www.yes24.com/Product/Goods/77125987?pid=123487&cosemkid=go15646485055614872&gclid=Cj0KCQiA0p2QBhDvARIsAACSOONw97-MO96DgAzl2A1bNi53QOCFceL8n6E2VRdPnxDE-U0Q-vD6Lj4aAggPEALw_wcB)
- https://www.eginnovations.com/blog/java-threads/
- https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism
- https://stackoverflow.com/questions/34689709/java-threads-and-number-of-cores/34689857#34689857
- https://javacan.tistory.com/entry/134
