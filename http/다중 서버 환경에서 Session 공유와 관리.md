# 세션 클러스터링

Spring Boot 에서 캐시를 기본적으로 지원한다.

반면 세션은 Cache 처럼 Spring 에서 지원해주는 기본 SessionStorage 는 없다.

하지만 우리는 Session 을 사용할 때 저장이 잘 된다는 것을 알 수 있다.

> [기본 세션 스토리지 없이 세션이 저장되서 사용 가능한 이유](https://stackoverflow.com/questions/57738669/what-is-the-default-session-storage-for-spring-boot)

답변을 보자면 바로 Spring Boot의 was의 기본값이 톰캣이다.

이 톰캣에서 `"SESSIONS.ser"` 파일에 `session의 상태가 저장` 된다고 한다.

보통 웹 개발을 하다보면 서버 이중화, 세션 클러스트링 이라는 말을 자주 듣는데, 일반적으로는 기존 서버(톰캣)이 가지고 있는 `로컬 세션 저장소`를 이용하는것 같다.

일반적으로 톰캣의 `all-to-all 세션 복제 방식`을 사용하는 것 같다. (primary-secondary 방식일 수도 있다.)

근데 이 방법은 정합성 이슈를 해결할 수는 있지만 성능적인 한계가 존재한다고 한다. 이러한 단점을 보완하고 다중 서버에서 세션을 공유하는 방법으로는

`세션 스토리지 분리`를 통해 정합성 이슈와 성능을 끌어 올릴 수 있다. 

> 세션 스토리지 분리 : 이는 기존 서버가 갖고 있는 로컬 세션 저장소를 이용하는 것이 아니라, `별도의 세션 저장소를 사용`하는 것을 의미한다.

웹 서비스 특성상 대부분 요청은 인가된 사용자가 보내는 요청인지 확인하는 절차가 필요하기 때문에 매번 세션 스토리지를 방문해야 하므로 읽기 속도가 중요하다.

따라서 빠르게 데이터를 찾아서 제공할 수 있는 데이터베이스를 사용해야하는데 

Disk 기반 DB vs In-Memory 기반 DB(Redis, Memcached) 이 있고

가장 성능이 뛰어나고 많이 사용하는건 Redis 이다.

## References.

> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 1편](https://hyuntaeknote.tistory.com/4?category=867120)
>
> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 2편(Sticky Session, Session Clustering, Session Storage 분리)](https://hyuntaeknote.tistory.com/6)
>
> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 3편 (Disk based database vs In-Memory database)](https://hyuntaeknote.tistory.com/7?category=867120)
>
> [다중 서버 환경에서 Session은 어떻게 공유하고 관리할까? - 4편(Redis vs Memcached)](https://hyuntaeknote.tistory.com/8?category=867120)
