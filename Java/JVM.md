# JVM(Java Virtual Machine) : CERG, MHSPN

- JVM
  - 자바 가상 머신으로, 자바 바이트 코드(class 파일)를 OS 에 특화된 코드로 변환(인터프리터와 JIT 컴파일러) 하여 실행
  - 바이트 코드를 실행하는 표준이자 구현체이다.
  - JVM 스펙 (https://docs.oracle.com/javase/specs/jvms/se11/html)
  - JVM 밴더 : 오라클, 아마존, Azul 
  - 자바 코드를 컴파일 해서 얻은 바이트 코드를 운영체제가 이해할 수 있는 기계어로 바꿔 실행
  - 플랫폼 종속적(OS 에 의존적)
  
## Architecture

> 자바 소스파일을 `Java Complier` 가 클래스 파일로 변환하고, `Class Loader` 가 `Runtime Data Area` 에 클래스 파일을 적재 시킨다. 
`Execution Engine` 이 자바 메모리에 적재된 클래스들을 기계어로 변환해 명령어 단위로 실행한다. 그리고 Garbage Collector 는 Heap 영역에 적재된 객체들 중에서 참조되지 않은 객체를 제거한다.

- JVM 은 크게 4가지로 구분(CERG)
  - `Class Loader (클래스 파일 적재 .. 실행 데이터 영역에 적재)`
    - 자바에서 소스를 작성하면 Person.java 처럼 .java파일이 생성된다. .java 소스를 자바컴파일러가 컴파일하면 Person.class 같은 .class 파일(바이트코드)이 생성된다. 
    이렇게 생성된 클래스파일들을 엮어서 JVM 이 운영체제로부터 할당받은 메모리영역인 `Runtime Data Area` 로 적재하는 역할을 Class Loader가 한다. (자바 애플리케이션이 실행중일 때 이런 작업이 수행된다.)
  - `Execution Engine (실행 엔진)`
    - Class Loader 에 의해 메모리에 적재된 클래스(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할을 한다. 명령어를 하나 하나 실행하는 인터프리터(Interpreter) 방식이 있고 JIT(Just-In-Time) 컴파일러를 이용하는 방식이 있다.
    JIT 컴파일러는 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Execution Engine이 네이티브로 컴파일된 코드를 실행하는 것으로 성능을 높이는 방식이다.
  - `Runtime Data Area (실행 데이터 영역, 자바 런타임 메모리, MHSPN)`
    - JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다.
    - Method Area(Class Area, Code Area, Static Area)
      - 클래스 멤버 변수 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보와 메서드 이름
      - 리턴 타입, 파라미터, Type 정보(interface 인지, class 인지), 상수 풀(문자상수, 타입 필드, 객체 참조), static 변수, final class 등
    - Heap Area
      - new 키워드로  생성된 객체와 배열
      - GC 의 대상
    - Stack Area
      - 지역변수, 파라미터, 리턴 값 연산에 사용되는 임시 값 등
      - Person p = new Person() 작성한 경우 p 는 스택 영역에 생성되고, 힙 영역의 주소값을 가지고 있다. new 로 생성된 Person 클래스는 힙 영역 생성된다.
      - 메서드를 호출할 때마다 개별적으로 스택이 생성된다.
    - PC Register
      - Thread(쓰레드)가 생성될 때마다 생성되는 영역으로 Program Counter 즉, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이(*CPU의 레지스터와 다름*) 이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.
    - Native method stack
      - 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역
  - `Garbage Collector(GC)`
    - GC 는 Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할을 한다. GC가 역할을 하는 시간은 정확히 언제인지를 알 수 없다. (참조가 없어지자마자 해제되는 것을 보장하지 않음) 또 다른 특징은 GC 가 수행되는 동안 GC 를 수행하는 쓰레드가 아닌 다른 모든 쓰레드가 일시정지된다.

> 쓰레드가 생성 되었을 때 메서드 영역과, 힙 영역은 모든 쓰레드가 공유하고 스택 영역, PC 레지스터, Native method stack 영역은 각각 스레드 마다 생성되고 공유되지 않는다. 따라서 동기화 작업을 할때, synchronized 없이 동시성 문제를 해결하고 싶은 경우 지역변수를 이용하는 방법이 있다.

# JRE(Java Runtime Environment) : JVM + 라이브러리

자바 런타임 환경은 JVM 에서 실행하기 위한 자바 애플리케이션을 실행할 수 있도록 구성된 배포판. JRE 는 자바 개발 키트를 다운로드할 때 기본적으로 포함되며 각 JRE 에는 코어 자바 클래스 라이브러리, 자바 클래스 로더, 자바 가상 머신이 포함된다.

- JVM 과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 세팅이나 리소스 파일을 가지고 있다.(JVM + 라이브러리)
- 개발 관련도구는 포함하지 않는다.(JDK 에서 제공)

## JDK(Java Development Kit) : JRE + 개발툴

- 자바 언어는 플랫폼 독립적(WORA : Write Once Run Anywhere)
- 자바 11 부터는 JRE 제공 안함
- 자바 9 부터 모듈 시스템 제공하기 때문에 JRE 를 구성할 수 있다.
- 오라클에서 만든 Oracle JDK 11 버전부터 상용으로 사용할 때 유료. 

## References.

> https://jeong-pro.tistory.com/148
>
> https://aboullaite.me/understanding-jit-compiler-just-in-time-compiler/ 
>
> https://howtodoinjava.com/java/basics/jdk-jre-jvm/ 
>
> https://en.wikipedia.org/wiki/List_of_JVM_languages 

