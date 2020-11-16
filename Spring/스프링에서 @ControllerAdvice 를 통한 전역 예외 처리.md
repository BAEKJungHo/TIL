# 스프링에서 @ControllerAdvice 를 통한 전역 예외 처리


스프링 부트가 아닌 스프링에서 @ControllerAdvice 가 동작하게 하기 위해서는 `<mvc:annotation-driven />` 를 추가해야한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:task="http://www.springframework.org/schema/task"
	   xmlns:mvc="http://www.springframework.org/schema/mvc"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
						http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.1.xsd
						http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
	   http://www.springframework.org/schema/mvc
		http://www.springframework.org/schema/mvc/spring-mvc-3.1.xsd">

	<context:component-scan base-package="egovframework">
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
		<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>

	<context:annotation-config/>	<!--  registered for JavaConfig -->

	<mvc:annotation-driven /> <!-- mvc annotation driven -->

    <!-- MULTIPART RESOLVERS -->
    <!-- regular spring resolver -->
    <bean id="spring.RegularCommonsMultipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="${file.upload.maxsize}" />
        <property name="maxInMemorySize" value="100000000" />
    </bean>

    <!-- choose one from above and alias it to the name Spring expects -->
    <alias name="spring.RegularCommonsMultipartResolver" alias="multipartResolver" />

	<!-- scheduler -->
	<task:annotation-driven/>
</beans>
```

만약 @RestControllerAdvice 를 지원하지 않는 경우 api 응답 예외 처리는 @ExceptionHandler 와 @ResponseBody 를 붙여야 한다.


```java
@ControllerAdvice
public class ExceptionHandler {

    /**
     * BusinessException 예외 처리
     * @param e
     * @return
     */
    @ResponseBody
    @org.springframework.web.bind.annotation.ExceptionHandler(BusinessException.class)
    public ResponseEntity businessException(BusinessException e) {
        return new ResponseEntity(e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }

    /**
     * DataAccessException 예외 처리
     * @param e
     * @return
     */
    @ResponseBody
    @org.springframework.web.bind.annotation.ExceptionHandler(DataAccessException.class)
    public ResponseEntity dataAccessException(DataAccessException e) {
        return new ResponseEntity(e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }

}
```
