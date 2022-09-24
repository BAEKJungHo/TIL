# Dockerfile

> Docker runs instructions in a Dockerfile in order.
>
> Docker 이미지를 만들기 위한 명령어를 포함하고 있는 파일

- A Dockerfile must begin with a `FROM` instruction
  - The FROM instruction specifies the Parent Image from which you are building
- globally scoped instruction is `ARG`
- comment instruction is `#`
- [WORKDIR](https://docs.docker.com/engine/reference/builder/#workdir)
  - The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile
  - If not specified, the default working directory is `/`
  - Therefore, to avoid unintended operations in unknown directories, it is best practice to set your WORKDIR explicitly
- [RUN](https://docs.docker.com/engine/reference/builder/#run)
  - The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile
- [COPY](https://docs.docker.com/engine/reference/builder/#copy)
  - ```
    COPY [--chown=<user>:<group>] <src>... <dest>
    COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
    ```
- [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint)
  - ```
    ENTRYPOINT ["executable", "param1", "param2"]
    ENTRYPOINT command param1 param2
    ```
    
## Example

```
FROM azul/zulu-openjdk:17.0.2
ARG JAR_FILE=./build/libs/*.jar
COPY ${JAR_FILE} order-app.jar
ENTRYPOINT ["java", "-jar", "order-app.jar"]
```

- FROM : 새로 생성할 image 의 기반으로 사용할 image 를 지정한다
  - azul/zulu-openjdk:17.0.2 image 를 포함한 image 생성
- COPY : 지정된 경로의 호스트 파일을 container 에 복사
  - build 된 jar 파일을 container 내부에 복사
- ENTRYPOINT : container 실행 시 추가로 수행할 실행 커맨드 입력
  - 복사한 order-app.jar 파일 실행

## Kafka with Dockerfile

```
# docker compose -f kafka.yml up -d
# docker compose -f kafka.yml build --no-cache
version: '3.9'
name: kafka
services:
  zookeeper:
    container_name: zookeeper
    image: bitnami/zookeeper:3.6.3
    ports:
      - "2181:2181"
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
    networks:
      - internal-network
  kafka:
    container_name: kafka
    image: bitnami/kafka:3.2.1
    ports:
      - "9092:9092"
    environment:
      - KAFKA_BROKER_ID=1
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://127.0.0.1:9092
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - ALLOW_PLAINTEXT_LISTENER=yes
    networks:
      - internal-network
    depends_on:
      - zookeeper

  app-delivery:
    container_name: delivery
    build: ../application/delivery
    ports:
      - 8081:8081
    networks:
      - internal-network
    depends_on:
      - kafka
  app-logging:
    container_name: logging
    build: ../application/logging
    ports:
      - 8082:8082
    networks:
      - internal-network
    depends_on:
      - kafka
  app-message:
    container_name: message
    build: ../application/message
    ports:
      - 8083:8083
    networks:
      - internal-network
    depends_on:
      - kafka
  app-order:
    container_name: order
    build: ../application/order
    ports:
      - 8084:8084
    networks:
      - internal-network
    depends_on:
      - kafka

networks:
  internal-network:
    driver: bridge
```

`docker compose -f kafka.yml build --no-cache` 를 통해 이미지 생성

`docker images` 를 통해 이미지 확인

```
docker run --name maru-spring -p 8080:8080 -d maru-spring
docker run --name '이미지 이름' -p '로컬포트:도커포트' -d 이미지이름 (-d는 백그라운드 실행을 의미)
```

## Links

- https://docs.docker.com/engine/reference/builder/
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- [How to Do a Clean Image Rebuild and Clear Docker's Cache](https://www.freecodecamp.org/news/docker-cache-tutorial/#:~:text=You%20can%20use%20the%20%2D%2D,in%20building%20your%20Docker%20container.)
- https://thalals.tistory.com/240
