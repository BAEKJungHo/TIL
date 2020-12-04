# 세션 클러스터링

Spring Boot 에서 캐시를 기본적으로 지원한다.

반면 세션은 Cache 처럼 Spring 에서 지원해주는 기본 SessionStorage 는 없다.

하지만 우리는 Session 을 사용할 때 저장이 잘 된다는 것을 알 수 있다.

> [기본 세션 스토리지 없이 세션이 저장되서 사용 가능한 이유](https://stackoverflow.com/questions/57738669/what-is-the-default-session-storage-for-spring-boot)

답변을 보자면 바로 Spring Boot의 was의 기본값이 톰캣이다.

이 톰캣에서 `"SESSIONS.ser"` 파일에 `session의 상태가 저장` 된다고 한다.


## References.

> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 1편](https://hyuntaeknote.tistory.com/4?category=867120)
>
> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 2편(Sticky Session, Session Clustering, Session Storage 분리)](https://hyuntaeknote.tistory.com/6)
>
> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 3편 (Disk based database vs In-Memory database)](https://hyuntaeknote.tistory.com/7?category=867120)
>
> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 4편(Redis vs Memcached)](https://hyuntaeknote.tistory.com/8?category=867120)
