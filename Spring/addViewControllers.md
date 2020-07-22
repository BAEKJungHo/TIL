# 스프링과 스프링 부트에서 컨트롤러 없이 바로 view 페이지로 매핑하는 방법

- 스프링 XML 버전

```xml
<mvc:view-controller path="/isYourURL" view-name="/View/Location"/>
```

- 스프링 부트 Java 버전

WebMvcConfigurerAdapter 를 상속받은 WebConfig 에서 구현하면 됨

```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
  registry.addViewController("/isYourURL").setViewName("/View/Location");
}
```
