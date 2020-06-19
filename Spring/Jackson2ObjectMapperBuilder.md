# Jackson2ObjectMapperBuilder

스프링 부트에서는 jackon 라이브러리를 지원한다. (srping-boot-starter-web 에 jackson-databind 가 있다.) 스프링은 따로 설정이 필요하다. 즉, json 변환을 위해서 jackson-databind 
의존성을 추가해야 한다.

Jackson2ObjectMapperBuilder 는 autoDetectGettersSetters(boolean autoDetectGettersSetter) 메서드를 사용하기 때문에

dto 객체를 json 으로 변환하고 싶으면 getter 와 setter 메서드를 꼭 만들어야 한다.

