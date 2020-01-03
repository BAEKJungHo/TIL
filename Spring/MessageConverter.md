## 메시지 컨버터

- 요청 본문에서 메시지를 읽어들이거나 , 응답본문에 메시지를 작성할 경우 사용한다.
- @RequestBody 
  - 요청 본문에 있는 메시지를 HttpMessageConverter를 사용해서 Conversion 한다.
- @ResponseBody 
  - 핸들러 메시지의 리턴값 (응답) 을 응답의 본문으로 넣어준다.
  
## MappingJackson2HttpMessageConverter

나는 `<, >` 등의 특수문자를 처리하는 작업을 Java Object를 JSON으로 변환시켜 주는 MappingJackson2HttpMessageConverter에 추가했고, HttpMessageConverter List에 추가시켰다. 

```java
@Configuration
public class HttpMessageConverterConfig extends WebMvcConfigurationSupport {

	@Bean
	public HttpMessageConverter<?> htmlEscapingConverter() {
		ObjectMapper objectMapper = new ObjectMapper();
		objectMapper.getFactory().setCharacterEscapes(new HTMLCharacterEscapes());
		objectMapper.registerModule(new JavaTimeModule());
		objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
		objectMapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

		MappingJackson2HttpMessageConverter htmlEscapingConverter =
				new MappingJackson2HttpMessageConverter();
		htmlEscapingConverter.setObjectMapper(objectMapper);

		return htmlEscapingConverter;
	}

	@Override
	protected void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
		converters.add(htmlEscapingConverter());
		super.addDefaultHttpMessageConverters(converters);  // default Http Message Converter  추가
	}
}
```

```java
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        super.configureMessageConverters(converters);
        converters.add(xssEscapeConverter());
    }

    private HttpMessageConverter<?> xssEscapeConverter () {
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.getFactory().setCharacterEscapes(new XSSEscapes());
        MappingJackson2HttpMessageConverter xssEscapeConverter =
                new MappingJackson2HttpMessageConverter();
        xssEscapeConverter.setObjectMapper(objectMapper);
        return xssEscapeConverter;
    }
```
  
## References

- [준영님 TIL](https://github.com/ces518/ILE/blob/master/spring/HttpMessageConverter.md)

- [yoojh9.github.io](https://yoojh9.github.io/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-HttpMessageConverter/)
