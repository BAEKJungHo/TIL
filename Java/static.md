# static은 언제 사용할까?

> https://baekjungho.github.io/java-static/

`public static`으로 선언된 메서드들은 순수함수(부수효과가 없는 함수)이다. 즉, 메서드들이 순수함수인 경우 public static을 사용할 수 있다.

- 유틸리티 클래스로 작성되고, 변화를 가정하지 않는다.
- 메서드가 인스턴스 변수를 사용하지 않는다.
- 인스턴스 생성에 의존하지 않는다.
- 메서드가 공유되고 있다면, 정적 메소드로 추출해낼 수 있다.
- 메서드가 변화되지 않고, 오버라이딩 되지 않는다.

 static이 실행되는 시점은 클래스가 메모리상에 올라갈 때이다.


## 장점

- static 을 사용하면 객체를 생성하지 않고 변수나 메서드에 접근하기 때문에 속도가 빠르다

## 단점

- static 변수들은 Method Area(Class Area, Code Area, Static Area) 에 생성되며, Static Area 는 가비지 컬렉션의 대상이 아니기 때문에, 메모리에서
지워지지 않습니다.
