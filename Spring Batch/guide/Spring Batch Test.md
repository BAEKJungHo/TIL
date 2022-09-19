# Spring Batch Test

- __TestBatchConfig__

```java
@ConfigurationPropertiesScan({"package.properties"})
@ComponentScan({"package.moduleName", "package.batch"})
@Configuration
@EnableAutoConfiguration
@EnableBatchProcessing
public class TestBatchConfig {
}
```

## Links

- https://jojoldu.tistory.com/455
