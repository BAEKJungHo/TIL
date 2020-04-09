# @PostConstruct

- 패키지: javax.annotation
- 버전: jdk1.6, spring 2.5
- 설정 위치: 초기화 작업 수행 메서드 앞
- 의존하는 객체를 설정한 이후에 초기화 작업을 수행하기 위해 사용한다.
스프링에 의해 인스턴스가 생성된 후 어노테이션이 적용된 메서드가 호출된다. 
사용하려면 `CommonAnnotationBeanPostProcessor` 클래스를 빈으로 등록시켜줘야 한다. `<context:annotation-config>` 태그로 대신할 수 있다.

```java
@PostConstruct
public void init() {
    System.out.println("객체 생성 후 내가 먼저 실행된다.");
}
```
