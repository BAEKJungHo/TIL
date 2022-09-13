# Spring Boot Cache

- [Caching Docs](https://docs.spring.io/spring-boot/docs/2.1.6.RELEASE/reference/html/boot-features-caching.html)

## with Redis

- https://www.baeldung.com/spring-boot-redis-cache

## with EhCache

ehcache 는 Spring 에서 간단하게 사용할 수 있는 Java 기반 오픈소스 캐시 라이브러리이다.

redis 나 memcached 같은 캐시 엔진들도 있지만, 저 2개의 캐시 엔진과는 달리 ehcache 는 데몬을 가지지 않고 Spring 내부적으로 동작하여 캐싱 처리를 한다.

따라서 redis같이 별도의 서버를 사용하여 생길 수 있는 네트워크 지연 혹은 단절같은 이슈에서 자유롭고 같은 로컬 환경 일지라도 별도로 구동하는 memcached 와는 다르게 ehcache 는 서버 어플리케이션과 라이프사이클을 같이 하므로 사용하기 더욱 간편하다.

- https://www.ehcache.org/
- https://medium.com/finda-tech/spring-%EB%A1%9C%EC%BB%AC-%EC%BA%90%EC%8B%9C-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-ehcache-4b5cba8697e0
- https://kimyhcj.tistory.com/253

## Links

- https://stackoverflow.com/questions/8181768/can-i-set-a-ttl-for-cacheable
- https://www.skyer9.pe.kr/wordpress/?p=1571
