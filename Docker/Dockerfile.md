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

## Links

- https://docs.docker.com/engine/reference/builder/
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- [How to Do a Clean Image Rebuild and Clear Docker's Cache](https://www.freecodecamp.org/news/docker-cache-tutorial/#:~:text=You%20can%20use%20the%20%2D%2D,in%20building%20your%20Docker%20container.)
