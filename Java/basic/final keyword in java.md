# final 에 대한 이해

final 키워드는 여러 컨텍스트에서 단 한번만 할당될 수 있는 entity 를 정의할 때 사용한다.

- final 변수
  - 원시 타입
  - 객체 타입
  - 클래스 필드
  - 메서드 인자
- final 메서드
- final 클래스

## final 변수

### 원시 타입

```java
@Test 
void test_final_primitive_variables() {
  final int num = 1;
  // num = 2; 불가능 한번 할당(assign)되면 변경할 수 없음.
}
```

### 객체 타입

객체 변수에 final 로 선언하면 그 변수에 다른 참조 값을 지정할 수 없다. 단 객체 자체가 `immutable`하다는 의미는 아니다. 객체의 속성은 변경 가능하다.

```java
@Test
void test_final_reference_variables() {
  final User user = new User();
  // user = new User();  // 다른 객체로 변경 불가능
  user.setName("BAEK"); // 필드 변경은 가능
}
```

### 메서드 인자

메서드 인자에 final 키워드를 붙이면, 메서드 안에서 원시 타입의 경우에는 값을 변경할 수 없고, 객체 타입의 경우에는 다른 참조 값을 지정할 수 없다.

```java
@Test
void test_final_method_arguments(final int age) {
  // age = 10; 불가능
}
```

### 멤버 변수

클래스의 맴버 변수에 final 로 선언하면 상수값이 되거나 write-once 필드로 한 번만 쓰이게 된다. final 로 선언하면 초기화되는 시점은 생성자 메서드가 끝나기 전에 초기화가 된다. 하지만, static 이냐 아니냐에 따라서도 초기화 시점이 달라진다.

- static final 맴버 변수 (static final int x = 1)
  - 값과 함께 선언시 
  - 정적 초기화 블록에서 (static initialization block)
  - 클래스 로드시 한번만 블록이 실행 됨
- instance final 맴버 변수 (final int x = 1)
  - 값과 함께 선언시 
  - 인스턴스 초기화 블록에서 (instance initialization block)
  - 생성자 메서드에서
  - 부모 생성자 이후에 실행되며, 생성자보다 먼저 실행됨
  - 객체를 생성할 때 마다 블록이 실행됨

## final 메서드

메서드를 final 로 선언하면 오버라이딩이 불가능하다. 즉, 메서드의 변경이 일어나지 않길 원할때, side-effect 가 있으면 안될 때 자주 사용한다.

## final 클래스

클래스를 final 로 선언하면 상속이 불가능하다. immutable 한 클래스를 만들고 싶거나, 주로 Util 성 클래스나 Constants 를 모아둔 클래스에서 자주 사용한다.

## References

> [final keyword in java](https://www.geeksforgeeks.org/final-keyword-java/?ref=rp)
