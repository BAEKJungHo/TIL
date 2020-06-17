# 직렬화(Serializable)

- 프록시 디자인 패턴 중
  - 인자와 리턴 값은 반드시 원시 형식(primitive) 또는 Serializable 형식으로 선언해야 한다.
 
 그 이유는, 모두 네트워크를 통해서 전달되기 때문에 직렬화를 통해 포장된다. 리턴값도 마찬가지이다. 원시 형식이나 API 에서 많이 사용하는 배열, 컬렉션형식을 사용한다면 걱정을 안해도 되지만, 직접 만든 형식일 경우 클래스를 만들 때 Serializable 인터페이스를 꼭 구현해야 한다.
 
> 즉, 직렬화(Serializable) 를 한다는 것은 네트워크 전송을 한다는 것이다.

- 직렬화 하고싶지 않은 필드의 경우에는 `transient` 키워드를 붙여야 한다.

```java
public class NoQuarterState implements State {

  /**
   * 직렬화하고 싶지 않은 필드의 경우 transient 키워드로 해결할 수 있다.
   * 하지만 객체를 직렬화 해서 전송받은 후에 이 필드를 호출하게되면 안 좋은 상황이 생길 수 있다.
   */
  transient GumballMachine gumballMachine;
}
```
