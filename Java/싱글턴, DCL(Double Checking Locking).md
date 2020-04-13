# 싱글턴에 사용되는 DCL(Double Checking Locking)

싱글턴을 구현할 때 멀티스레딩 환경에서도 동작가능하게끔 구현해야한다. 그러려면 동시성문제를 생각해야하는데, 객체를 생성해주는 메서드에 동기화
블럭을 지정하는 방법이 있을 수 있다. 코드는 아래와 같다.

> 만약 동기화 블럭을 지정하지 않는다면, 객체가 변조될 것이다. 왜냐하면 static 키워드가 붙은 애들은 메서드(클래스) 영역에 메모리가 등록되는데, 이 영역은
힙 영역과 마찬가지로 공유 가능한 자원이기 때문이다.

```java
public class Singleton {
  private static Singleton uniqueInstance;
  
  priavte Singleton() {}
  
  // lazy initialization
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

다른 방법은 `Eager initialization 이른 초기화 방식`, 클래스 로딩시 즉 static 키워드의 특징을 이용(JVM 클래스 로더 시스템의 로딩, 링크 초기화 과정 중에서 초기화 과정에서 메모리에 등록됨) 하여
인스턴스를 처음에 미리 생성하는 것이다.


```java
public class Singleton {
  // 클래스 로딩 시점에서 생성
  private static Singleton uniuqeInstance = new Singleton();
  
  private Singleton() {}
  
  public static Sigleton getInstance() {
   return uniqueInstance;
  }
  
}
```

다음 방법은 DCL 을 이용하는건데 자바 1.4 버전 이상부터 사용가능하다. 즉, 객체가 생성되지 않은 경우에만 동기화 블럭이 실행되게끔 하는 것이다.

```java
public class Singleton {
  private volatile static Sigleton uniqueInstance;
  
  private Singleton() {}
  
  // lazy initialization : 클래스 로딩 시점이 아닌 인스턴스가 필요하여 요청할 때 생성되는 형태로 작성하였다.
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

> volatile 키워드를 사용하면 멀티스레딩을 쓰더라도 uniqueInstance 변수가 Sigleton 인스턴스로 초기 되는 과정이 올바르게 진행되도록 할 수 있다.

다음은 `Enum` 을 이용하는 방법이 있다.

Enum 은 인스턴스가 여러개 생기지 않도록 보장해주며, 직렬화가 자동으로 지원된다. 단점은 Enum 은 컴파일 시점에 성격이 결정되어, 매번 메서드를 호출할 때 Context 정보를 넘겨야하므로 비효율적인 상황이 생길 수 있다.

```java
public enum Singleton {
  INSTANCE;
}
```

마지막 방법은 `LazyHolder` 라고 내부클래스를 사용하여 인스턴스를 생성하는 것인데, 미리 인스턴스를 생성하는 방법과 유사하다. 현재 가장 많이 사용하는 방식이며, volatile 이나 synchronized 키워드가 없어도 동시성 문제를 해결한다. 따라서 성능도 뛰어나다.

LazyHolder 방식은 static 객체임에도 불구하고 필요시 메모리에 할당해서 쓴다는 것과(동적바) 그 방식이 쓰레드 안전성을 보장해준다는 겁니다.

```java
public class Singleton {

    //private construct
    private Singleton() {}

    /**
     * 내부 클래스의 유형 중에서 static 내부 클래스(중첩클래스)만 static멤버를 가질 수 있다. (정식명칭은 static member class 정적 멤버 클래스)
     * 거의 쓰이지 않지만 내부클래스에서 static변수를 선언해야하는 경우 static 내부 클래스를 선언해야만 한다.
     * static 멤버, 특히 static메서드에서 사용될 목적으로 선언
     */
    private static class InnerInstanceClazz() {
        // 클래스 로딩 시점에서 생성
        private static final Singleton uniqueInstance = new Singleton();
    }

    public static Singleton getInstance() {
        return InnerInstanceClazz.instance;
    }
    
}
```

JVM의 클래스 로더 메커니즘과 클래스의 로드 시점을 이용하여 내부 클래스를 통해 생성 시킴으로써 쓰레드 간의 동기화 문제를 해결한다.
위 방법은 현재 java에서 싱글톤 생성에서 사용하는 대표적인 방법이다.

Singleton 클래스에는 InnerInstanceClazz() 클래스의 변수가 없기 때문에, static 멤버 클래스더라도, 클래스 로더가 초기화 과정을 진행할때 InnerInstanceClazz 메서드를 초기화 하지 않고, getInstance () 메서드를 호출할때 초기화 된다. 즉, `동적바인딩(Dynamic Binding)` 런타임시에 성격이 결정 되는 특징을 이용하여 thread-safe 하면서 성능이 뛰어나다.

InnerInstanceClazz 내부 인스턴스는 static 이기 때문에 클래스 로딩 시점에 한번만 호출된다는 점을 이용한것이다.. 또 final을 써서 다시 값이 할당되지 않도록 합니다.

> 내부 클래스에 관해서 이펙티브 자바 참고

## 싱글턴 사용시 주의사항

- 클래스 로더를 2개 이상 사용하는 경우 객체가 2번 이상 생성될 수 있다.
  - 이럴때는 클래스 로더를 지정해야함
  
> 자바와 Spring에서의 싱글톤 차이점이라면, 싱글톤 객체의 생명주기가 다르다. 또한 자바에서 공유 범위는 Class loader 기준이지만, Spring 에서는 ApplicationContext 가 기준이 된다.  

## 스프링 applicationContext 와 싱글톤

스프링은 빈을 등록할 때 범위를 지정할 수 있는데 Default 가 Singleton 이다. 

싱글톤 말고도 session, request, prototype와 같은 값들로 설정이 가능하다.

- 프로토타입 스코프 : 싱글톤스코프와는 다르게 컨테이너에 빈을 요청할 때마다 매번 새로운 오브젝트만든다.
- 리퀘스트 스코프 : 웹을 통해 새로운 HTTP 요청이 생길때마다 생성된다.

Root Application Context , Application Context 안에 들어 있는 객체는 Thread safe 입니다.

예를들어 VO 가 Root Application Context에 있을 경우, 다른 여러 요청이 같은 VO에 접근하면 이전 요청과 간섭이 발생하여 올바른 데이터를 얻지 못할 것입니다.

즉. 각 컨테이너의 객체는 싱글톤이며, Thread safe 해야 합니다.

```java
/**
 * 스프링에서는 주로 ServiceImpl 을 빈으로 등록하기 위해서 @Service 를 붙인다. 즉, SerivceImpl 은 thread-safe 하다
 * 반면 serviceImpl 내부 메서드 중 동시성 처리를 위해 동기화 블럭을 지정해야하는 메서드가 있을 수 있는데 해야하는 이유는, 동시에 메서드에 접근한 경우 DB에 값이 2번 쓰여질 수도 있다 즉, PK 는 달라도 필수값(Unique key 로 지정해야할 필요가 있는 값) 들이 2번 쓰여지게 됩니다.(테이블 에 LOCK 을 걸거나, Unique key 지정 등을 하지 않은경우)
 * 즉, Thread-safe 한 경우라도 위와같은 경우에는 synchronized 블럭을 지정해야하기도 한다.
 */
``` 
Thread safe하기 위해서는 객체에 인스턴스 변수가 없으면 됩니다.

나중에 코드를 보시면 아시겠지만 DAO , Controller는 외부에서 접근 가능한 멤버 변수가 없었습니다.

그러나 VO 는 getter , setter로 멤버 변수 조작이 가능하죠.

Spring 은 디폴트로 내부에서 생성하는 Bean object 를 singleton으로 만든다.
이것은 디자인 패턴의 싱글톤과 비슷한 개념이지만, java 의 일반 구현방법은 확연히 다르다.
즉, Spring container 에서 object 를 singleton registry 에 관리하며 singleton으로 동작하도록 제어한다.

`Spring의 ApplicationContext 는 Singleton을 저장하고 관리하는 Singleton registry 이다.`

스프링의 핵심 컨테이너의 빈 관리를 담당하는 BeanFactory 의 핵심 구현 클래스는 `DefaultListableBeanFactory`이다. 대부분의 애플리케이션 컨텍스트는 바로 이 클래스를 BeanFactory 로 사용하는데, 이 DefaultListableBeanFactory 가 구현하고 있는 인터페이스의 한가지가 바로 SingletonRegistry 이다. 결국 `스프링 컨테이너 = 애플리케이션 컨텍스트 = 빈 팩토리 = 싱글톤 레지스트리` 가 되는 것이다.

### 스프링이 왜 bean 을 singleton 으로 생성하는지?

하나의 요청(Request)를 처리하기 위해서는 데이타 액세스 로직(DAO), 서비스 로직, 비즈니스 로직, 프리젠테이션 로직등 다양한 기능을 담당하는 오브젝트들이 계층형 구조로 이루어 진다. 매번 클라이언트 요청이 올때마다, 각 로직을 담당하는 오브젝트들을 새로 만들어 사용한다면, 아무리 Java가 오브젝트 생성과 GC(Garbage Collection) 성능이 좋아졌어도 서버가 부하를 감당하기 들다. 이 때문에 엔터프라이즈 분야에서는 서비스오브젝트 라는 개념을 사용해 왔는데 Servlet 은 Java 엔터프라이즈 기술의 가장 기본이 되는 서비스 오브젝트라고 할수 있다. Servlet 은 대부분 멀티스레드 환경에서 Singleton 으로 동작한다. 즉, Servlet 클래스 당 하나의 오브젝트만 만들어 주고, 사용자의 요청을 담당하는 여러 스레드에서 하나의 오브젝트를 공유해서 사용한다.

* Singleton registry(applicationContext) 덕분에 singleton 으로 동작해야 할 어떤 클래스라도 평범한 java class 로 작성될 수 있다. 즉, public 생성자를 가질 수 있으며 static method 를 노출할 필요도 없다. 이것은 테스트 환경에서 자유롭게 object 를 만들 수 있고 또한 OOP의 장점을 그대로 가진다.

> 즉, 스프링 applicationContext 는 IoC컨테이너 이면서 SingletonRegistry 이다.

### private 생성자를 가진 클래스도 스프링 빈으로 등록이 가능할까?

private 생성자를 가진 클래스도 스프링의 빈으로 바로 사용이 가능하다 이다. 스프링은 리플렉션을 통해서 인스턴스를 만들고, 리플렉션을 통해서라면 private생성자를 호출해서 인스턴스를 만드는 것이 가능하다. 해보면 알겠지만 스프링은 별 경고 없이 그냥 빈을 만들어버린다.

하지만 그다지 권장하고싶지 않다. 가능하면 클래스 설계자의 의도를 존중하고, 접근방법을 지키도록 하는 것이 바람직하다.

### 스프링 싱글턴 설계시 주의점

Spring 에서 singleton 이 멀티스레드 환경에서 서비스 오브젝트에 사용되는 경우에는 상태정보를 내부에 갖고 있지 않은 stateless 로 만들어야 한다. 이 경우, 각 요청에 대한 정보나 생성된 정보는 메서드 파라미터나 메서드 안의 로컬변수 혹은 리턴값 등으로 처리해야 한다. __싱글톤이라해도 메서드 파라미터나, 메서드 안에서 생성되는 로컬변수는 메서드가 호출될 때마다 매번 새로 할당되므로__ 싱글톤이라 해도 여러 스레드가 변수의 값을 덮어쓸 일은 없다.

```java
public class UserDao {
  private ConnectionMaker connectionMaker; <-- (1)
  private Connection c;  <-- (2)
  private User user;       <-- (2)

  public User get(String id) throws ClassNotFoundException, SQLException {
    this.c = connectionMaker.makeConnection();
    ....
    this.user = new User();
    this.user.setId("...");
    ...
    return user;
  } 
```

UserDao 는 IoC container 에 의해서 singleton object 로 생성/사용된다고 가정해 보자.
이런 경우, (2)번과 같이 멀티스레드 환경에서 변경될 수 있는 값을 클래스의 인스턴스 변수로 생성하는 경우, 문제가 발생한다. 따라서 Spring 의 singleton bean 으로 사용되는 클래스를 만들때는 (2)번의 경우는 method 내 로컬 변수로 사용하거나 파라미터로 주고 받으며 사용해야 한다.

단, 이 경우에도 클래스 인스턴스 변수로 사용가능한 경우가 있는데 이는 (1) 과 같다.
(1)은 자신이 사용하는 다른 singleton bean 을 저장하려는 용도라면, 인스턴스 변수로 사용해도 좋다. 스프링이 초기화 되고 나면, 이후에 수정되지 않는 경우라면, 멀티스레딩 환경에서 사용해도 문제없다

## 사용되는 예

- 레지스트리 같은 인스턴스가 꼭 1개만 있어야 하는 경우
- 스프링에서 @Repository 를 등록하면 Root Application Context, 즉 bean Factory에 해당 객체가 싱글톤 형태로 저장 된다.
  
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

> https://itmore.tistory.com/entry/스프링-ApplicationContext-동작방식
>
> https://nesoy.github.io/articles/2018-06/Java-volatile
>
> https://victorydntmd.tistory.com/161
>
> https://joont.tistory.com/144
>
> https://medium.com/@joongwon/multi-thread-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%EC%9D%98-%EC%98%AC%EB%B0%94%EB%A5%B8-singleton-578d9511fd42
>
> http://toby.epril.com/?p=849
