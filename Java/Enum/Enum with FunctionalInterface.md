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
