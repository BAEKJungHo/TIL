# Java8 Permgen 영역 삭제

자바 개발자라면 `OutOfMemoryError: PermGen Space error` 이 발생했던것을 본적이 있을텐데 이는 Permanent Generation 영역이 꽉 찼을때 발생하고 메모리 누수가 발생했을때 생기게 된다.
메모리 누수의 가장 흔한 이유중에 하나로 메모리에 로딩된 클래스와 클래스 로더가 종료될때 이것들이 가비지 컬렉션이 되지 않을때 발생한다.

- Java 7 의 Metaspace
  - PermGen 은 Java 8 부터 Metaspace 로 완벽하게 대체 되었고 Metaspace 는 클래스 메타 데이터를 native 메모리에 저장하고 메모리가 부족할 경우 이를 자동으로 늘려준다.
  - Java 8 의 장정줌에 하나로 OutOfMemoryError: PermGen Space error 는 더이상 볼 수 없고 JVM 옵션으로 사용했던 PermSize 와 MaxPermSize 는 더이상 사용할 필요가 없다. 
  - 이 대신에 MetaspaceSize 및 MaxMetaspaceSize 가 새롭게 사용되게 되었다. 이 두 값은 Metaspace 의 기본 값을 변경하고 최대값을 제한 할 수 있다.

> Perm 영역은 보통 Class의 Meta 정보나 Method의 Meta 정보, Static 변수와 상수 정보들이 저장되는 공간으로 흔히 메타데이터 저장 영역이라고도 한다. 
> 이 영역은 Java 8 부터는 Native 영역으로 이동하여 Metaspace 영역으로 변경되었다. (다만, 기존 Perm 영역에 존재하던 Static Object 는 Heap 영역으로 옮겨져서 GC의 대상이 최대한 될 수 있도록 하였다)

- 왜 Permgen 영역이 삭제되고 Metaspace 영역이 추가된 것 일까?

최근 Java 8에서 JVM 메모리 구조적인 개선 사항으로 Perm 영역이 Metaspace 영역으로 전환되고 기존 Perm 영역은 사라지게 되었다. Metaspace 영역은 Heap 이 아닌 `Native 메모리 영역`으로 취급하게 된다. (`Heap 영역은 JVM에 의해 관리`된 영역이며, `Native 메모리는 OS 레벨에서 관리`하는 영역으로 구분된다) Metaspace가 Native 메모리를 이용함으로서 개발자는 영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.
즉, 각종 메타 정보를 OS가 관리하는 영역으로 옮겨 Perm 영역의 사이즈 제한을 없앤 것이라 할 수 있다.

## 장점

- PermGen 영역이 삭제되어 heap 영역에서 사용할 수 있는 `메모리가 늘어났다.`
- PermGen 영역을 삭제하기 위해 존재했던 여러 복잡한 코드들이 삭제 되고 PermGen 영역을 스캔 하기 위해 소모되었던 시간이 감소되어 `GC 성능이 향상` 되었다.
- OutOfMemoryError: PermGen Space error 가 더이상 발생하지 않는다.

## References

> https://johngrib.github.io/wiki/java8-why-permgen-removed/
> 
> https://starplatina.tistory.com/entry/JDK8%EC%97%90%EC%84%A0-PermGen%EC%9D%B4-%EC%99%84%EC%A0%84%ED%9E%88-%EC%82%AC%EB%9D%BC%EC%A7%80%EA%B3%A0-Metaspace%EA%B0%80-%EC%9D%B4%EB%A5%BC-%EB%8C%80%EC%8B%A0-%ED%95%A8
