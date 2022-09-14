# Job, Step, Tasklet

> 하나의 Job 에서 수행될 Step(단계) 들을 지정하고, 각 Step 에서는 Tasklet 이 존재

![스크린샷 2022-09-14 오후 2 03 49](https://user-images.githubusercontent.com/47518272/190063973-a142fcb3-94b6-45ad-8bf1-394f0311616c.png)

- Job
  - 하나의 배치 작업 단위를 의미
  - 여러 Step이 존재할 수 있음
- Step
  - Step 안에 Tasklet 혹은 Reader & Processor & Writer 묶음이 존재
- Tasklet
  - Step 안에서 수행될 기능들을 명시
  - Tasklet 은 Step 안에서 단일로 수행될 커스텀한 기능들을 선언할때 사용

> Tasklet 하나와 Reader & Processor & Writer 한 묶음이 같은 레벨. 그래서 Reader & Processor가 끝나고 Tasklet으로 마무리 짓는 등으로 만들순 없다는걸 꼭 명심해야 함
