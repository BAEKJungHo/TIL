# N 개의 배치서버가 하나의 metadata table 을 공유해도 되는지

- N 개의 배치서버가 하나의 데이터베이스 메타데이터 테이블을 바라봐도 문제가 없는지 ? (= 서로 다른 배치 작업들이 서로 다른 시스템에서 실행 되더라도 동일한 데이터베이스를 바라봐도 문제가 없는지)
  - __Sequence__
    - BATCH_JOB_INSTANCE, BATCH_JOB_EXECUTION, BATCH_STEP_EXECUTION
    - 핵심 테이블들의 ID 채번 방식이 시퀀스이기 때문에 PK 채번 과정에서 오류가 발생 X
  - __OptimisticLocking__
    - 다수의 테이블에 version 컬럼이 존재
    - Spring Batch 공식 문서에서 다음과 같이 설명하고 있음
      - This check is necessary, since, even though different batch jobs may be running in different machines, they all use the same database tables. 
  - __BATCH_JOB_INSTANCE = 최상위 계층에 속하는 핵심 테이블__
    - 문제가 발생할 수도 있을만한 상황?
      - JOB_NAME 이 같으면?
        - Spring Batch 공식문서: Name of the job obtained from the Job object. Because it is required to __identify the instance__, it must not be null.
        - JOB_NAME 은 인스턴스를 식별하기 위한 키값
        - JOB_NAME 이 같더라고 JOB_PARAMETER 가 다르면 JOB_KEY 컬럼 값이 달라져서 문제가 되진 않음. 단, JOB_PARAMETER 까지 같은 경우에는  JobInstanceAlreadyCompleteException 에러가 발생할 수 있음  

따라서, JOB_NAME 이 중복되지만 않으면 문제가 발생 X. 또한 일반적으로 JOB_NAME 이 같기는 어렵다고 생각되기 때문에 N 개의 배치서버가 하나의 metadata table 을 공유해도 문제가 없을 거라 생각    

## Links

- https://docs.spring.io/spring-batch/docs/current/reference/html/schema-appendix.html#metaDataSchema
- https://jojoldu.tistory.com/326
