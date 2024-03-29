# MySQL vs MongoDB

- MySQL 은 Multi-thread 로 query 를 처리하는데, MongoDB는 Single-thread 로 query 들을 queue 에 쌓아놓고 하나씩 실행한다.

# Is MongoDB always faster then MySQL?

> https://blog.tinned-software.net/is-mongodb-always-faster-then-mysql/

## MySQL 은 쿼리를 어떻게 처리합니까?

MySQL 은 꽤 오래된 데이터베이스 개념을 기반으로 합니다. 
그렇기 때문에 처리해야 할 몇 가지 문제가 있습니다. 
MySQL 의 역사를 생각하면 구현의 세부 사항이 그대로 수행되는 이유를 설명할 수 있습니다.
즉, MySQL 은 성능을 향상시키기 위해 다중 스레드를 사용하는 것과 같은 최신 기술도 사용합니다.
여러 쿼리를 한 번에 처리해야 하는 경우 여러 스레드를 사용하여 쿼리를 병렬로 작업합니다.
따라서 많은 수의 쿼리가 있는 높은 로드 상황이 있는 경우 MySQL 은 이러한 쿼리 중 일부를 동시에 처리합니다.

_쿼리가 데이터를 쓰는 동안 충돌을 피하기 위해 MySQL 은 잠금 이라는 기술을 사용하여 쓰기 액세스를 차단하고 동일한 항목에 대한 쓰기 액세스를 번갈아 가면서 보호합니다._

사용하는 스토리지 엔진에 따라 행 잠금 이라는 메커니즘이 사용될 수있습니다. 
행 잠금을 사용하면 쿼리가 행에 데이터를 쓰는 경우 잠금 메커니즘이 첫 번째 쓰기 프로세스가 완료될 때까지 테이블의 동일한 행에 대한 다른 쓰기 액세스를 차단합니다.
동시에 다른 쿼리는 이 테이블의 다른 행에 있는 데이터를 변경할 수 있지만 현재 쓰고 있는 행은 변경할 수 없습니다. 

## MongoDB 는 쿼리를 어떻게 처리합니까?

MongoDB 의 내부 개념은 MySQL 과 완전히 다릅니다. 여기에서 그것들을 비교하면서 나는 한 가지 중요한 차이점을 지적하고 나머지는 무시하고 싶습니다.

_MongoDB 에서 쿼리는 동시에 처리되지 않습니다._ 

즉, MongoDB 서버로 전송되는 모든 쓰기 쿼리가 대기열에 추가됩니다. 서버는 각 쿼리를 하나씩 처리합니다.
(작성하는 순간, 쿼리가 동일한 컬렉션[SQL 용어 테이블], 데이터베이스로 전송되는지 여부는 차이가 없습니다).
MongoDB는 [instance-wide-locking](https://www.mongodb.com/docs/manual/faq/concurrency/#what-type-of-locking-does-mongodb-use) 이라는 기술을 사용합니다.

_이것은 전체 서버 프로세스가 한 번에 하나의 쓰기 쿼리만 실행할 수 있음을 의미합니다._

이 병목 현상을 약간 제거하기 위해 MongoDB 는 현재 최신 릴리스 노트에서 언급한 것처럼 `데이터베이스 전체 잠금`으로 전환하고 있습니다.

이것이 의미하는 바는 무엇입니까? 인스턴스 전체 잠금을 사용하면 전체 MongoDB 서버가 한 번에 하나의 쓰기 쿼리만 처리할 수 있습니다. 
데이터베이스 전체 잠금 을 사용하면 MongoDB 가 데이터베이스당 하나의 쓰기 쿼리를 처리할 수 있지만 동시에 여러 데이터베이스를 사용할 수 있습니다.
그것은 당신에게 끔찍하게 들릴지 모르지만 MongoDB 의 쿼리가 MySQL 보다 훨씬 빠르게 처리된다는 것을 기억하면 이것이 큰 문제가 아니라는 것을 알게 될 것입니다. 
물론 이 모든 것은 데이터베이스와 컬렉션을 올바르게 설정한 경우에만 적용됩니다!

* Single write lock (per database)
* Reader-Writer lock :  allowing concurrent access to multiple threads for reading but restricting access to a single thread for writes (or other changes) to the resource

(from wikipedia) In computer science, a readers-writer or shared-exclusive lock (also known as the multiple readers / single-writer lock[1] or the multi-reader lock,[2] or by typographical variants such as readers/writers lock) is a synchronization primitive that solves one of the readers-writers problems. A readers-writer lock is like a mutex, in that it controls access to a shared resource, allowing concurrent access to multiple threads for reading but restricting access to a single thread for writes (or other changes) to the resource. A common use might be to control access to a data structure in memory that can't be updated atomically and isn't valid (and shouldn't be read by another thread) until the update is complete.

## MongoDB 는 MySQL과 어떻게 비교됩니까?

단일 쿼리의 성능 면에서 MongoD B는 MySQL 보다 훨씬 빠릅니다. 
내 경험에 따르면 일부 특수한 상황에서 과부하 상황에서 차이점을 알 수 있습니다. 
MySQL 에서 모든 유능한 데이터베이스 관리자는 인덱스의 중요성을 알고 있어야 합니다. 
MongoDB 와 비교할 때 대부분의 관리자는 인덱스와 같은 것이 있다는 것조차 모르는 것 같습니다!
그것은 MongoDB 가 구조가 없는 데이터베이스라는 사실에서 비롯된 것일 수 있습니다.
그렇다면 내가 왜 신경을 써야 할까요? 그러나 여기서 적절한 인덱스 사용이 적어도 다른 데이터베이스 엔진만큼 중요하다는 점을 지적하고 싶습니다.

## MongoDB 에서 인덱스는 얼마나 중요합니까?

MongoDB는 쓰기 쿼리에 대해 인스턴스 전체 잠금 또는 `데이터베이스 전체 잠금` 을 사용하므로 MySQL 에서보다 인덱스를 적절하게 설정하는 것이 훨씬 더 중요할 수 있습니다.
내 경험의 예를 보여 드리겠습니다. 이 경우 여전히 instance-wide-locking 을 사용하고 있다고 가정하겠습니다.

MongoDB 데이터베이스와 MySQL 데이터베이스 모두에서 각 항목의 길이가 몇 만 개의 항목이 있는 테이블이 있다고 상상해 보십시오. 
현재 정의된 인덱스가 없습니다. 필드 중 하나의 특정 필드 값에 따라 약 100 개의 항목을 업데이트하기 위해 쿼리를 실행하면 어떻게 될까요?

MySQL 은 하나의 스레드에서 쿼리 처리를 시작하고 업데이트할 행을 찾기 위해 수백만 개의 항목을 모두 검색하기 위해 "전체 테이블 스캔"을 수행해야 합니다.
이 쿼리는 우리가 상상하는 환경에서 약 20분 정도 걸릴 것입니다.

MongoDB는 약 5분 안에 동일한 쿼리를 실행합니다. 
이 소리는 훌륭하지 않습니까? 
사실, MongoDB 서버는 업데이트를 위한 관련 항목을 찾기 위해 전체 컬렉션(SQL 용어 테이블에서)을 검색해야 합니다. 
앞에서 논의한 바와 같이 이 5분 동안 MongoDB 는 쓰기 액세스를 위해 전체 서버 프로세스를 잠급니다. 
5분이 더 빠르게 들린다 고 생각할 수도 있습니다. 그렇지 않나요? 그래서 MongoDB가 더 빠르기 때문에 더 낫습니까?

시스템에 대한 전체 영향과 비교한 단일 쿼리의 시간은 두 가지입니다.
MySQL 은 동일한 데이터베이스나 테이블에 대해서도 데이터베이스 서버에 대한 다른 쓰기 쿼리를 계속 처리합니다. 
서버가 "장기 실행 쿼리"로 바쁜 동안 리소스가 사용되고 다른 쿼리에 대해 여유가 없기 때문에 이러한 다른 쿼리는 약간 느리게 실행될 수 있지만 여전히 처리됩니다.

그러나 MongoDB 에서 일어나는 일은 눈을 뜨게 할 수 있습니다.
5분 쿼리가 MongoDB 에서 실행되는 동안 인스턴스 전체 잠금활성. 이렇게 하면 예외 없이 다른 모든 쓰기 쿼리가 대기열에 추가됩니다.
모두 5분 쿼리가 완료될 때까지 기다린 다음 하나씩 실행됩니다.
시간 초과 또는 기타 이유로 인해 기본 연결이 이미 포기된 경우에도 마찬가지입니다. 
나는 다른 이유 외에도 여기에서 잠금의 주요 이유는 일관성이라고 생각합니다.
이 예에서 단일 쿼리에 대해 5분이라는 긴 쿼리 실행 시간으로 인해 애플리케이션에서 시간 초과가 발생할 수 있습니다.
예를 들어, PHP 스크립트에는 스크립트가 종료된 후 시간 초과가 있으며, 특히 웹 응용 프로그램의 경우 웹 브라우저는 5분보다 훨씬 짧은 시간 초과 시간을 가지며
이 시간이 지나면 서버에 대한 연결이 끊어진 것으로 간주합니다.

언급했듯이 대기열에 있는 쿼리는 클라이언트에서 MongoDB 서버로의 연결이 이미 닫혀 있더라도 대기열에 남아 있고 도착한 순서대로 실행됩니다.

두 시스템 모두에서 적절하게 정의된 인덱스는 쿼리 실행 시간을 크게 줄일 수 있습니다.

## MongoDB Update

MySQL과 마찬가지로. Insert, Update, Delete 쿼리는 Index들을 재구성하기 때문에 신중해야 합니다.
특히 Update는 maximum-size  초과 시에 기존 doc을 지우고 재 생성하여 디스크에 기록하기 때문에 성능저하는 당연히 발생할 수 밖에 없습니다.

`max-size: power of 2 size (2의 제곱 크기)`

Update로 record의 크기가 36MB 를 넘어섰다면 32 * 2 = 64MB의 공간을 확보하여 새로운 Record를 작성한다.

## 결론

MongoDB 가 단일 단순 쿼리에서 훨씬 더 빠르게 수행하더라도; 이것이 항상 더 빠르다는 의미는 아니며 이와 같은 구조가 없는 데이터베이스에도 일종의 조직이 필요합니다.
인덱스 정의는 매우 중요한 작업입니다.
각 쿼리의 실행 시간이 MySQL 보다 짧더라도 누락된 인덱스는 큰 영향을 미칠 수 있습니다.

MongoDB 개발 로드맵 에서 알 수 있듯이
설명된 문제는 개발자가 알고 있는 것이며 인스턴스 전체 잠금 에서 데이터베이스 전체 잠금 으로의 전환은 올바른 방향으로 가는 큰 단계입니다. 
더 유용한 기능이 곧 제공되기를 바랍니다.

모든 응용 프로그램에 가장 적합한 만능 데이터베이스 엔진이 있을 수 있다고 생각하는 사람을 위해 한 가지만 말씀드리겠습니다. 
모든 작업에 적합한 도구가 있고 모든 도구에 적합한 도구가 있습니다. 
사용하는 곳입니다. 이것은 데이터베이스 엔진뿐만 아니라 다른 도구, 소프트웨어에도 적용됩니다!

## References

- https://www.mongodb.com/docs/manual/faq/concurrency/
- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=joebak&logNo=220337489706
