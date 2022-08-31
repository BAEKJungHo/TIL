# 분산락

- 여러 서버를 운영하는 분산환경에서는 한 서버에 락이 걸려있더라도 다른 서버로 동일한 요청이 가게 된다면 동기화를 보장할 수 없다.
- 분산 락은 데이터베이스 등 공통된 저장소를 이용하여 자원이 사용 중인지를 체크한다. 그래서 전체 서버에서 동기화된 처리가 가능해진다.
- 여러 요청마다 락을 점유하고 데이터 업데이트 하기 때문에 각 서버는 각 DB의 동기화를 기다리지 않아도 되며, 동시성 문제도 해결할 수 있다.
  - Ex. 수량이라는 자원


## Redisson 분산락

- __RLock 사용__
  - tryLock(...)
    - 파라미터로 들어오는 leaseTime 시간 동안 락을 점유하는 시도합니다.
    - 락을 사용할 수 있을 때까지 waitTime 시간까지 기다립니다.
    - leaseTime 시간이 지나면 자동으로 락이 해제됩니다.
    - 정리하자면 선행 락 점유 스레드가 존재하면 waitTime동안 락 점유를 기다리며 leaseTime 시간 이후로는 자동으로 락이 해제되기 때문에 다른 스레드도 일정 시간이 지난 후 락을 점유할 수 있습니다.

## Links

- https://hyperconnect.github.io/2019/11/15/redis-distributed-lock-1.html
- https://velog.io/@hgs-study/redisson-distributed-lock
