# MapStruct 

MapStruct 는 객체간 (Entity(VO) DTO) 매핑을 지원해주는 라이브러리다.

클라이언트에서 보낸 요청 본문이 VO 에 담기면, VO 에서 필요한 값만 DTO 저장한다 던지, 엔티티와 클라이언트쪽으로 이동하는 DTO 간에 일반적인 유형의 변환이 발생하는데
이러한 문제를 해결해주는 도구이다. 엔티티와 DTO 간의 필드명이 달라도 매핑이 가능하다.

- 타입 세이프하게 bean 매핑을 도와주는 어노테이션 프로세서
- 리플랙션을 사용하지 않기 때문에 매핑 속도가 빠름
- 도메인 객체를 풍부하게 사용하면서, 반환 데이터가 달라지게 될 경우 이를 적절하고 큰 힘을 들이지 않고 매핑할 수 있도록 도와주는 것이다.

@Builder 나 vo, dto 간 변환을 위해서 각 클래스에 메서드를 선언하게 되면, entity 클래스에서 프로퍼티가 추가되면 클래스 2개, 메서드 2개를 수정해야 하므로 복잡하고 헷갈린다.

MapStruct 는 `어노테이션 프로세서` 를 사용하여 @Mapper 어노테이션이 붙은 인터페이스를 알아서 구현해준다.

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

- EntityMapper interface (최상위)

```java
public interface EntityMapper<D, E> {
    E toEntity(D dto);
    D toDto(E entity);
}
```

- 각 프로그램 기능에 맞게 프로그램Mapper interface 생성
    - 만약에, 자바 버전이 낮아 MapStruct 어노테이션 프로세서를 사용 못하면 구현체도 직접 생성

```java
public xxxMapper implements EntityMapper {}
```

```java
@Component
public class xxxMapperImpl implements xxxMapper {

    @Override
    public Object toEntity(Object dto) {
        xxxDto to = (xxxDto) dto;
        if(dto == null) {
            return null;
        }
        xxxVo vo = new xxxVo();
        vo.setId(to.getId());
        vo.setParent(to.getParent());
        vo.setText(to.getText());
        return vo;
    }

    @Override
    public Object toDto(Object entity) {
        xxxVo vo = (xxxVo) entity;
        if(entity == null) {
            return null;
        }
        xxxDto dto = new xxxDto();
        dto.setId(vo.getId());
        dto.setParent(vo.getParent());
        dto.setText(vo.getText());
        return dto;
    }

}
```

## References.

> https://mapstruct.org/
>
> https://www.baeldung.com/mapstruct
>
> https://mapstruct.org/documentation/1.0/reference/html/#implicit-type-conversions
>
> https://meetup.toast.com/posts/213
