# Amazon Aurora DB 클러스터가 장애 조치된 후 읽기 전용 오류가 발생하는 이유는 무엇입니까?

> The MySQL server is running with the --read-only option so it cannot execute this statement

Amazon Aurora DB 클러스터에서 다중 AZ 장애 조치가 발생하면 클러스터 엔드포인트가 자동으로 업데이트되어 라이터 및 리더의 새로 지정된 역할을 반영하고 이를 가리킵니다. 이전 라이터가 재부팅된 다음 기존 복제본이 라이터로 승격되는 동안 읽기 전용 모드로 설정됩니다.

읽기 전용 오류 메시지가 나타나면 이는 리더 역할의 기존 노드를 통해 데이터 정의 언어(DDL), 데이터 조작 언어(DML) 또는 데이터 제어 언어(DCL) 작업을 수행하려고 함을 의미합니다. Aurora MySQL에서 다음과 같이 innodb_read_only 변수를 확인하여 이를 확인할 수 있습니다.

## 다중 AZ 장애 조치에 대한 근본 원인 분석을 수행하고 Amazon RDS 인스턴스를 다시 시작하려면 어떻게 해야 합니까?

- https://aws.amazon.com/ko/premiumsupport/knowledge-center/rds-multi-az-failover-restart/
- [Multi-AZ deployments for high availability](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html#Concepts.MultiAZ.Failover)

## 방안

- https://aws.amazon.com/ko/premiumsupport/knowledge-center/aurora-mysql-db-cluser-read-only-error/
- Writer instance 접속시 Instance Endpoint 가 아닌 Cluster Endpoint 를 사용하도록 Application 에서 관리
  - 클러스터 엔드포인트는 DB 클러스터에 대한 읽기/쓰기 연결 시 장애 조치를 지원
  - https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.Endpoints.html
