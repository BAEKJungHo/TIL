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
    
- `@ConstructorProperties`
   - 생성자의 속성 명칭 지정
   - https://multifrontgarden.tistory.com/222 : 롬복 버전이 Jackson Deserialize 에 어떤 영향을 미치는지
   
- `@Builder`
  - 빌더 패턴을 쉽게 적용할 수 있다.
  - `@Builder.Default` 기본값을 적용할 수 있다.
  - 출처 : https://tomining.tistory.com/180
  - @Builder 적용 X
  ```java
    public class User {
    private String name;
    private int age;

    public static UserBuilder builder() {
      return new UserBuilder();
    }

    public String getName() {
      return name;
    }

    public void setName(String name) {
      this.name = name;
    }

    public int getAge() {
      return age;
    }

    public void setAge(int age) {
      this.age = age;
    }
  }

  //Builder Class
  public class UserBuilder {
    private String name;
    private int age;

    public User build() {
      User user = new User();
      user.setName(this.name);
      user.setAge(this.age);
      return user;
    }

    public UserBuilder name(String name) {
      this.name = name;
      return this;
    }

    public UserBuilder age(int age) {
      this.age = age;
      return this;
    }
  }
  
  public class UserBuilderTest {
    @Test public void builderTest() {
      User user = User.builder()
                    .name("홍길동")
                    .age(19)
                    .build();
      System.out.println(user);
    }
  }
  ```
  
  - @Builder 적용
  ```java
  @Data
  @Builder
  public class User {
    private String name;
    private int age;
  }

  public class UserBuilderTest {
    @Test  public void builderTest() {
      User user = User.builder()
                    .name("홍길동")
                    .age(19)
                    .build();
      System.out.println(user);
    }
  }
  ```

  - @Builder 사용시 특정 속성에 기본값을 설정하려면?
    - @Builder.Default 는 롬복 1.16.16 이상 부터 가능 
    - 그 이전 버전은? 즉, @Builder를 사용한 경우 build()시 설정하지 않으면 0 / null / false가 된다고 언급하고 있다.

  ```java
  @Data
  @Builder
  public class User {
    private String name;
    @Builder.Default  private int age = 19;
  }
  
  public class UserBuilderTest {
    @Test  public void builderTest() {
      User user = User.builder()
                    .name("홍길동")
                    .build();
      System.out.println(user);
    }
  }
  ```
  
  


