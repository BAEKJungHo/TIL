# Array, ArrayList, LinkedList 사용시 for vs forEach 속도 차이

```java
List<String> list1 = new ArrayList<>();
List<String> list2 = new LinkedList<>();
```

위 처럼 List 타입으로 받는 이유는 다형성 때문이다. 다형성을 이용해 구현체에 상관 없게끔 코드를 짜는 것이다. 

arrayList 의 경우는 for 문이 forEach 보다 미세하게 속도가 빠르다. 만약 위 코드에서 list1 의 구현체가 무조건 ArrayList 인 경우는 for 문을 사용하여
검색하는게 더 성능상 좋지만, 만약 반환 타입이 LinkedList 로 변경되거나, 구현체가 바뀔 수도 있는 경우에는 forEach 를 사용하는게 낫다.

왜냐하면 arrayList 를 제외하고 나머지는 forEach 가 성능상 더 좋다. LinkedList 의 get(index) 메서드로 값을 검색하는 경우 get 메서드 내부에 for 문이 돌기 때문에 일반 for 문을 사용하면 복잡도가 2배 증가한다. 반면 forEach 를 사용하면 반복문 하나만 돌기 때문에 성능이 더 좋게 나온다.

- ArrayList 내부 

```java
E elementData(int index) {
    return (E) elementData[index];
}
```    

- LinkedList 내부 for 문

```java
/**
 * Returns the (non-null) Node at the specified element index.
 */
Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```   

> https://siyoon210.tistory.com/99
>
> https://multifrontgarden.tistory.com/130
