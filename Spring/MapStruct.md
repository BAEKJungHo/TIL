# MapStruct 

MapStruct 는 객체간 (Entity, VO, DTO) 매핑을 지원해주는 라이브러리다.

클라이언트에서 보낸 요청 본문이 VO 에 담기면, VO 에서 필요한 값만 DTO 저장한다 던지, 엔티티와 클라이언트쪽으로 이동하는 DTO 간에 일반적인 유형의 변환이 발생하는데
이러한 문제를 해결해주는 도구이다. 엔티티와 DTO 간의 필드명이 달라도 매핑이 가능하다.

- 타입 세이프하게 bean 매핑을 도와주는 어노테이션 프로세서
- 리플랙션을 사용하지 않기 때문에 매핑 속도가 빠름
- 도메인 객체를 풍부하게 사용하면서, 반환 데이터가 달라지게 될 경우 이를 적절하고 큰 힘을 들이지 않고 매핑할 수 있도록 도와주는 것이 바로 mapStruct

## 사용방법

- 의존성 추가

```xml
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-jdk8</artifactId>
    <version>1.3.0.Beta2</version> 
</dependency>
```

- mapstruct 프로세서는 빌드시 매퍼 구현을 생성하는 데 사용된다.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.5.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <annotationProcessorPaths>
            <path>
                <groupId>org.mapstruct</groupId>
                <artifactId>mapstruct-processor</artifactId>
                <version>1.3.0.Beta2</version>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>
```

## References.

> https://mapstruct.org/
>
> https://www.baeldung.com/mapstruct
>
> https://mapstruct.org/documentation/1.0/reference/html/#implicit-type-conversions
>
> https://meetup.toast.com/posts/213
