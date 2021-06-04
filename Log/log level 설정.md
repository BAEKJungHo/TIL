# 톰캣에서 쿼리 로그 안찍히게 하는 방법

- Log level 을 DEBUG 나 INFO 로 하면 실행한 쿼리가 찍힌다.
- Log level 을 ERROR 로 하면 ERROR 발생시에만 로그가 찍힌다.

> 로그 레벨은 회사마다 상황마다 알맞게 설정하면 된다.

- logback.xml

```java
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-3level %logger{5} - %msg %n</pattern>
        </encoder>
    </appender>

    <logger name="jdbc" level="OFF"/>

    <logger name="jdbc.sqlonly" level="OFF"/>
    <logger name="jdbc.sqltiming" level="ERROR"/>
    <logger name="jdbc.resultsettable" level="ERROR"/>
    <logger name="jdbc.audit" level="OFF"/>
    <logger name="jdbc.resultset" level="OFF"/>
    <logger name="jdbc.connection" level="OFF"/>

    <root level="INFO">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```
