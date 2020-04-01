# static은 언제 사용할까?

> https://baekjungho.github.io/java-static/

`public static`으로 선언된 메서드들은 순수함수(부수효과가 없는 함수)이다. 즉, 메서드들이 순수함수인 경우 public static을 사용할 수 있다.

- 유틸리티 클래스로 작성되고, 변화를 가정하지 않는다.
- 메서드가 인스턴스 변수를 사용하지 않는다.
- 인스턴스 생성에 의존하지 않는다.
- 메서드가 공유되고 있다면, 정적 메소드로 추출해낼 수 있다.
- 메서드가 변화되지 않고, 오버라이딩 되지 않는다.

final static 으로 선언하면 적어도 1 바이트 이상의 GC 대상 객체가 사라지게 된다. 또한 JNDI 이름이나 간단한 코드성 데이터들을 static 으로 선언해 놓으면 편리하다. static 이 실행되는 시점은 클래스가 메모리상에 올라갈 때이다.(클래스 로딩 시에 멤버가 생성된다. 객체가 생성되기 전에 생성된다.)

## main 메서드가 static 인 이유

메인 클래스의 static 을 해석할 때는 static 을 사용하면 객체를 생성하지 않고 메소드를 호출할 수 있다는 점에 초점을 맞추면 됩니다. 메인 메소드 호출은 JVM 에서 호출되기 때문에 객체 생성없이 메모리에 할당시켜 호출 가능한 형태로 만들어야하기 때문입니다. cmd 창에서 java 메인클래스 메소드를 이용해 사용하면 JVM 은 메인 클래스의 객체를 생성하는 것이 아니라 클래스의 static 으로 선언된 메소드를 객체 생성없이 메모리에 할당시키고 할당된 메소드 중 "main으로 네이밍된 메소드를 찾아 호출하게 되는 것입니다. 그렇기 때문에 메인 메소드는 무조건 static 으로 정의되어 있어야하는 것입니다. 

## 장점

- static 을 사용하면 객체를 생성하지 않고 변수나 메서드에 접근하기 때문에 속도가 빠르다
- static 변수, static 메서드만으로 이루어진 클래스가 있다면, 그것은 인스턴스를 만들어 사용하는 클래스보다 훨씬 빠르게 작동할 수 있다.

## 단점

- static 변수들은 Method Area(Class Area, Code Area, Static Area) 에 생성되며, Static Area 는 가비지 컬렉션의 대상이 아니기 때문에, 메모리에서
지워지지 않습니다.
- 만약 지속적으로 해당 객체에 데이터가 쌓인다면, 더이상 GC가 되지 않으면서 시스템은 `OutOfMemoryError` 를 발생시킬 것입니다.

## 의견

FileUtils 클래스 같은경우 전부다 static 메서드로 되어있던데  `public static List<FileVo> parseFileVoList(FileDto fileDTO, FileStore constants, List<String> extensionList)` 이런식으로 매개변수에 인스턴스변수 쓰는 애들이나, 인스턴스에  의존하는 경우에는 static 을 쓰면 안된다고 알고 있습니다. static 쓰면 GC(가비지 컬렉션) 대상이 아니라 메모리에 남아있어서 잘못하면 OOM 발생하고(static 에 대해서 정확히 알지 못하고 쓰는경우) 그러면 서버 다운됩니다. 대신 장점이 static 을 쓰면 속도가 빨라진다는 측면이 있는데 (FileUtils 에 있는 메서드들은 그렇게 속도 빨라야할 대상은 아님) 메모리 용량이 되게 크거나, 속도 측면과 맞바꿔야 할정도면 잘 판단해서 사용해야합니다. FileUtils 에서 static 으로 써도 될만한 애들은 `private static String generateFileName(String keyStr, int fileKey)` 이런 애들 입니다. 저희가 주로 enum 클래스들에 있는 메서드 들은 static 으로 사용하는데 메서드들이 일단 원시 타입을 매개변수로 받거나 매개변수가 없는 경우이며 부수효과가 없기 때문에 사용해도 되는 케이스 입니다.
