# ResourceLoader

ResourceLoader는 리소스를 읽어오는 기능을 제공하는 인터페이스다. ApplicationContext 인터페이스는 이 ResourcePatternResolver 인터페이스를 상속받은 상태이고 ResourcePatternResolver 는 ResourceLoader 를 상속 받는다. 따라서 ApplicationContext를 통해서도 ResourceLoader가 제공하는 메서드를 사용하는 것이 가능하다.

> [Spring docs](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/io/ResourceLoader.html)
>
> Strategy interface for loading resources (e.. class path or file system resources). An ApplicationContext is required to provide this functionality, plus extended ResourcePatternResolver support.
DefaultResourceLoader is a standalone implementation that is usable outside an ApplicationContext, also used by ResourceEditor.
Bean properties of type Resource and Resource array can be populated from Strings when running in an ApplicationContext, using the particular context's resource loading strategy.

- Must support fully qualified URLs, e.g. "file:C:/test.dat".
- Must support classpath pseudo-URLs, e.g. "classpath:test.dat".
- Should support relative file paths, e.g. "WEB-INF/test.dat". (This will be implementation-specific, typically provided by an ApplicationContext implementation.)

```java
// classPath 로 파일 찾기
Resource resource = resourceLoader.getResource("classpath:jXlsTemplate/survey.xlsx");

// file 키워드로 파일 찾기
Resource resource = resourceLoader.getResource("file:"+ "프로젝트 경로" + "/WEB-INF" + "파일경로");

// 상대경로로 파일 찾기
Resource resource = resourceLoader.getResource("WEB-INF/test.dat");
```
