# 스프링에서 Scheduler 설정하는 방법

- context-common.xml 에 task xsd 및 task:annotation-driven 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:task="http://www.springframework.org/schema/task"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
						http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.1.xsd
						http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

  <!--  registered for JavaConfig -->
	<context:annotation-config/>	
  	
	<!-- scheduler -->
	<task:annotation-driven/>
</beans>

```

- @Scheduled 가 선언되어있는 클래스는 빈으로 등록이 되어있어야 한다. (ex) @Component)
