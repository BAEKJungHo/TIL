# tomcat 서버 재기동 방법

- 톰캣이 떠 있는지 확인 + 위치 찾기
  - `ps -ef | grep tomcat`  
- 해당 위치로 이동 후 로그 확인
  - `tail -f ./catalina.out`  
- 내리기
  - ~/tomcat/apache*/bin 이동
  - `./shutdown.sh`
  - ps -ef | grep tomcat 으로 톰캣이 떠 있는지 확인
  - `kill -9 길게 떠 있는 로그 옆의 숫자`
- 올리기
  - ~/tomcat/apache*/bin 이동
  - `./startup.sh`
  
# 개발서버 띄울 때 해야할 일 들

- server.xml 에서 domain 에 해당하는 host 코드 작성
- vhost 에서 JK... 톰캣 버전 
