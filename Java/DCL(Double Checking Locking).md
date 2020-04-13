# 싱글턴에 사용되는 DCL(Double Checking Locking)

싱글턴을 구현할 때 멀티스레딩 환경에서도 동작가능하게끔 구현해야한다. 그러려면 동시성문제를 생각해야하는데, 객체를 생성해주는 메서드에 동기화
블럭을 지정하는 방법이 있을 수 있다. 코드는 아래와 같다.

> 만약 동기화 블럭을 지정하지 않는다면, 객체가 변조될 것이다. 왜냐하면 static 키워드가 붙은 애들은 메서드(클래스) 영역에 메모리가 등록되는데, 이 영역은
힙 영역과 마찬가지로 공유 가능한 자원이기 때문이다.

```java
public class Singleton {
  private static Singleton uniqueInstance;
  
  priavte Singleton() {}
  
  public static synchronized Singleton getInstance() {
    if(uniqueInstance != null) {
      uniqueInstance = new Singleton();
    }
    return uniqueInstance;
  }
}
```

이렇게 되면 uniqueInstance 가 존재하지 않는 경우에만 객체를 생성한다. 단점은 uniqueInstance 가 존재하더라도 동기화 블럭이 실행된다.

일반적으로 메서드에 동기화 블럭을 지정하면 성능이 100배 가량 떨어진다. 만약 getInsatnce() 속도가 중요하지 않다면 그냥 둬도 된다.

다른 방법은 클래스 로딩시 즉 static 키워드의 특징을 이용(JVM 클래스 로더 시스템의 로딩, 링크 초기화 과정 중에서 초기화 과정에서 메모리에 등록됨) 하여
인스턴스를 처음에 미리 생성하는 것이다.


```java
public class Singleton {
  private static Singleton uniuqeInstance = new Singleton();
  
  private Singleton() {}
  
  public static Sigleton getInstance() {
   return uniqueInstance;
  }
}
```

마지막 방법은 DCL 을 이용하는건데 자바 1.4 버전 이상부터 사용가능하다. 즉, 객체가 생성되지 않은 경우에만 동기화 블럭이 실행되게끔 하는 것이다.

```java
public class Singleton {
  private volatile static Sigleton uniqueInstance;
  
  private Singleton() {}
  
  public static Sigleton getInstance() {
    if(uniqueInstance == null) {
      synchronized (Singleton.class) {
        if(uniqueInstance == null) {
          uniqueInstance = new Singleton();
        }
      }
    }
    return uniqueInstance;
  }
}
```

> volatile 키워드를 사용하면 멀티스레딩을 쓰더라도 uniqueInstance 변수가 Sigleton 인스턴스로 초기ㅗ하 되는 과정이 올바르게 진행되도록 할 수 있다.

## 싱글턴 사용시 주의사항

- 클래스 로더를 2개 이상 사용하는 경우 객체가 2번 이상 생성될 수 있다.
  - 이럴때는 클래스 로더를 지정해야함

## 사용되는 예

- 레지스트리 같은 인스턴스가 꼭 1개만 있어야 하는 경우
  
## volatile

- volatile keyword는 Java 변수를 Main Memory 에 저장하겠다라는 것을 명시하는 것입니다.
- 매번 변수의 값을 Read 할 때마다 CPU cache 에 저장된 값이 아닌 Main Memory 에서 읽는 것입니다.
- 또한 변수의 값을 Write 할 때마다 Main Memory 에 까지 작성하는 것입니다.

### 필요한 이유?

volatile 변수를 사용하고 있지 않는 MultiThread 어플리케이션에서는 Task 를 수행하는 동안 성능 향상을 위해 Main Memory 에서 읽은 변수 값을 CPU Cache 에 저장하게 됩니다.
만약에 `Multi Thread 환경에서 Thread 가 변수 값을 읽어올 때 각각의 CPU Cache 에 저장된 값이 다르기 때문에` 변수 값 불일치 문제가 발생하게 됩니다.

volatile 키워드를 추가하게 되면 Main Memory 에 저장하고 읽어오기 때문에 변수 값 불일치 문제를 해결 할 수 있습니다. 즉, volatile 는 변수의 read 와 write 를 Main Memory 에서 진행하게 됩니다.


- Multi Thread 환경에서 하나의 Thread 만 read & write 하고 나머지 Thread 가 read 하는 상황에서 가장 최신의 값을 보장합니다.
- 하나의 Thread 가 아닌 여러 Thread 가 write 하는 상황에서는 적합하지 않습니다.
- 여러 Thread 가 write 하는 상황이라면?
  - synchronized 를 통해 변수 read & write의 원자성(atomic) 을 보장해야 합니다.

## References.

> https://nesoy.github.io/articles/2018-06/Java-volatile
