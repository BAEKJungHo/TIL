# elastic beanstalk spring health check

- yml 설정

```yml
management:
  endpoints:
    web:
      exposure:
        include:
          - health
```

- WebSecurity 에서 web.ignoring() 

## Links

- https://stackoverflow.com/questions/47189609/spring-boot-application-on-elastic-beanstalk-health-check-fails
