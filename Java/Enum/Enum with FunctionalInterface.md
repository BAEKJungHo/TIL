# Enum with FunctionalInterface

### Enum

```java
public enum PathType {
    DISTANCE(Section::getDistance),
    DURATION(Section::getDuration),
    ;

    private PathCostFunction pathCostFunction;

    PathType(PathCostFunction pathCostFunction) {
        this.pathCostFunction = pathCostFunction;
    }

    public int cost(Section section) {
        return pathCostFunction.cost(section);
    }
}
```

### FunctionalInterface

```java
@FunctionalInterface
public interface PathCostFunction {
    int cost(Section section);
}
```

### Section

```java
@Entity
public class Section {
  // 생략
  
  public int getDuration() {
    return duration;
  }
}
```

### Using

```java
private void addEdge(SimpleDirectedWeightedGraph<Station, SectionEdge> graph, PathType pathType) {
    // 지하철 역의 연결 정보(간선)을 등록
    lines.stream()
            .flatMap(it -> it.getSections().stream())
            .forEach(it -> {
                SectionEdge sectionEdge = SectionEdge.of(it);
                graph.addEdge(it.getUpStation(), it.getDownStation(), sectionEdge);
                graph.setEdgeWeight(sectionEdge, pathType.cost(it));
            });
}
```

## 요금 조회 

```java
import java.util.Arrays;
import java.util.function.Predicate;

public enum FareType {

    BASIC(distance -> distance <= 10, 0),
    PER_FIVE(distance -> distance > 10 && distance <= 50, 5),
    PER_EIGHT(distance -> distance > 50, 8);

    private static final int BASIC_FARE = 1_250;

    private final Predicate<Integer> predicate;
    private final int per;

    FareType(Predicate<Integer> predicate, int per) {
        this.predicate = predicate;
        this.per = per;
    }

    public static FareType from(int distance) {
        return Arrays.stream(FareType.values())
                .filter(fareType -> fareType.match(distance))
                .findFirst()
                .orElse(BASIC);
    }

    private boolean match(int distance) {
        return predicate.test(distance);
    }

    public static int fare(int distance) {
        return from(distance).calculate(distance);
    }

    private int calculate(int distance) {
        if (this == BASIC) {
            return BASIC_FARE;
        }

        int extraCharge = (int) ((Math.ceil(((double) distance - 11) / per) + 1) * 100);
        return BASIC_FARE + extraCharge;
    }

}
```
