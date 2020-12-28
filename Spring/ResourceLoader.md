# ResourceLoader

```java
// classPath 로 파일 찾기
Resource resource = resourceLoader.getResource("classpath:jXlsTemplate/survey.xlsx");

// file 키워드로 파일 찾기
Resource resource = resourceLoader.getResource("file:"+ "프로젝트 경로" + "/WEB-INF" + "파일경로");
```
