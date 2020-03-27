# 롬복(Lombok)

## 어노테이션 종류

- `@UtilityClass`
  - A utility class is a class that is just a namespace for functions. No instances of it can exist, and all its members are static. For example, java.lang.Math and java.util.Collections are well known utility classes. This annotation automatically turns the annotated class into one.
  A utility class cannot be instantiated. By marking your class with @UtilityClass, lombok will automatically generate a private constructor that throws an exception, flags as error any explicit constructors you add, and marks the class final. If the class is an inner class, the class is also marked static. All members of a utility class are automatically marked as static. Even fields and inner classes.

- `@RequiredArgsConstructor`
  - 초기화 되지않은 final 필드나, @NonNull 이 붙은 필드에 대해 생성자를 생성해 준다.
  - 의존성 주입(Dependency Injection) 할 때 사용
  - 어떠한 빈(Bean)에 생성자가 오직 하나만 있고, 생성자의 파라미터 타입이 빈으로 등록 가능한 존재라면 이 빈은 @Autowired 어노테이션 없이도 의존성 주입이 가능하다.
  
  ```java
  @RequiredArgsConstructor
  @Service
  public class RequiredArgsConstructorDIServiceExample {
    private final FirstRepository firstRepository;
    private final SecondRepository secondRepository;
    private final ThirdRepository thirdRepository;
    
    // ...
  }
  ```
  
  자바 파일이 위 처럼 되어있을경우 class 파일은 아래와 같이 만들어 진다.
  
  ```java
  @Service
  public class RequiredArgsConstructorDIServiceExample {
    @ConstructorProperties({"firstRepository", "secondRepository", "thirdRepository"})
    public RequiredArgsConstructorDIServiceExample(FirstRepository firstRepository, SecondRepository secondRepository, ThirdRepository thirdRepository) {
      this.firstRepository = firstRepository;
      this.secondRepository = secondRepository;
      this.thirdRepository = thirdRepository;
    }
  }
  ```
  
  따라서 빈으로 등록 가능한 파라미터 타입들에 대해 의존성 주입을 해준다.
    
