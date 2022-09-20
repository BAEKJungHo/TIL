# Dockerfile

> Docker runs instructions in a Dockerfile in order.

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

## Links

- https://docs.docker.com/engine/reference/builder/
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
