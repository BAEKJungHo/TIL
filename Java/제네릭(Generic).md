# 제네릭(Generic)

## 제네릭의 도입

JDK 1.5 버전 부터 제네릭이 도입되었다.

## 제네릭 이란?

제네릭(Generic)은 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미한다. 

제네릭(Generic)은 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입체크를 해주는 기능이다.

> 따라서, 동적(런타임시에)으로 타입을 결정할 수 있고, 넘겨진 인자의 타입이 일치하지 않는경우 타입 체킹을 통해 컴파일시에 오류를 발견할 수 있게 해준다.

## 제네릭의 장점

- 강력한 타입 체킹으로 인한 컴파일시에 에러 검출 가능
- 자동 캐스팅

## 제네릭을 사용하지 않는 경우

ArrayList 를 예를 들어 설명하겠다.

```java
public class MyArrayList {
    private int size;
    private Object[] elementData = new Object[5];

    public void add(Object value) {
        elementData[size++] = value;
    }

    public Object get(int idx) {
        return elementData[idx];
    }
}
```

```java
public class MyArrayListTest {
    public static void main(String[] args) {
        MyArrayList list = new MyArrayList();

        list.add("50"); 
        list.add("100"); 

        // ClassCastException
        Integer value1 = (Integer) list.get(0);
        Integer value2 = (Integer) list.get(1);

        // Casting 을 직접 해줘야 한다.
        String str1 = (String) list.get(0);
        String str2 = (String) list.get(1);
        
        System.out.println(value1 + value2);
    }
}
```

위 처럼 String 으로 넣고 Integer 로 파싱하게 되는 경우 `ClassCastException` 이 발생한다. 더 심각한 문제는 이러한 에러를 컴파일시에는 발견되지 않는다는 것이다. 또한
캐스팅을 직접해줘야한다.

위 코드를 제네릭으로 바꾸면 다음과 같다.

```java
public class MyArrayList<T> {
    private int size;
    private Object[] elementData = new Object[5];

    public void add(T value) {
        elementData[size++] = value;
    }

    public T get(int idx) {
        return (T) elementData[idx];
    }
}
```

`<T>` 로 표현한 것이 제네릭이다. MyArrayList 는 객체를 생성할때 타입을 지정하면, 생성되는 오브젝트 안에서는 T 의 위치에 지정한 타입이 대체되어서 들어가는 것 처럼 컴파일러가 인식한다. 
좀 더 정확하게 말하면, Raw 타입 으로 사용하는데 컴파일러에 의해 필요한 곳에 형변환 코드가 추가된다. (List<String> 을 List 로만 쓰는 것이 Raw 타입으로 사용하는 것이다)

사용은 아래처럼 하면된다.

```java
class Test {
    public static void main(String[] args) {
        MyArrayList<Integer> intList = new MyArrayList<>();
        intList.add(1);
        intList.add(2);

        int intValue1 = intList.get(0); // 형변환이 필요없다
        int intValue2 = intList.get(1); // 형변환이 필요없다

        // String strValue = intList.get(0); // 컴파일에러
    }
}
```

위 코드를 디컴파일하면 다음과 같다.

```java
class Test {
    Test() {
    }

    public static void main(String[] var0) {
        MyArrayList var1 = new MyArrayList(); // 제네릭이 사라졌다. RawType 으로 만 사용
        var1.add(1);
        var1.add(2);
        int var2 = (Integer)var1.get(0); // 형변환이 추가되었다
        int var3 = (Integer)var1.get(1); // 형변환이 추가되었다
    }
}
```

`MyArrayList<Integer>` 로 생성했던 타입파라미터가 사라지고, Raw 타입으로만 사용하는데, 
값을 꺼내 쓰는 곳에 형변환 코드가 추가되었다. 제네릭을 사용하면 컴파일러가 형변환을 알아서 진행한다는 것을 확인했다.

## 타입 추론(Type Inference)

JDK 1.7 버전 부터 제네릭에서 타입 추론이 가능하게 되었다. 따라서 타입 파라미터를 두번 명시하지 않아도 된다.

- (JDK 1.6) List<Integer> list = new ArrayList<Integer>(); 
- (JDK 1.7) List<Integer> list = new ArrayList<>();

## 한정적 타입 매개변수(Bounded Type Parameter)

## 와일드카드(WildCard)

## 제네릭을 사용할 수 없는 경우

## 제네릭 메서드

## References.

> https://yaboong.github.io/java/2019/01/19/java-generics-1/
>
> https://palpit.tistory.com/665
