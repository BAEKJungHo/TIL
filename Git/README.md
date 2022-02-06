# Forking Workflow 방식으로 협업하기

- __중앙 원격 리포지토리를 fork 한다.__
  - 중앙 원격 리포지토리의 브랜치 상태
    - master 가 있으며, 각각 사람 이름에 해당하는 branch 가 존재(Ex. baekjungho)
  - 자신의 깃허브를 가면 fork 된 리포지토리가 존재한다.
- __fork 된 리포지토리를 clone 한다.__
  - 터미널 : `git clone -b {본인_아이디} --single-branch https://github.com/{본인_아이디}/{저장소 아이디}`
    - Ex. `git clone -b javajigi --single-branch https://github.com/jungho/java-racingcar`
  - clone 을 완료하면 나의 Working Directory 에는 master 가 아닌 내 이름에 해당하는 브랜치만 가져오게된다.
- __새로운 브랜치 생성__
  - 터미널 : `git checkout -b 브랜치이름`
    - Ex. `git checkout -b step1`
- __add, commit, push__
  - 작업 후 add, commit, push 한다.
  - 이때 `upstream` 이 아닌 `origin`을 선택한다. 
    - 보통 upstream 은 중앙 원격 리포지토리를 의미하고 origin 은 fork 된 나의 리포지토리를 의미한다.
- __Pull Request__
  - GitHub GUI 를 통해서 PR 을 날린다.
- __중앙 원격 리포지토리에서 MERGE 된 이후__
- __기존에 작업했던 브랜치를 삭제한다.__
  - 터미널 : `git checkout 본인_아이디`
    - `git fetch <remote>`
    - Ex. git branch -D 삭제할_브랜치이름
- __중앙 저장소와 동기화를 위해 중앙 저장소 추가__
  - git remote add 명령은 최초 1회만 진행
  - `git remote add {저장소_별칭} base_저장소_url`
    - Ex. `git remote add upstream https://github.com/next-step/java-racingcar.git`
  - 위와 같이 next-step 저장소를 추가한 후 전체 remote 저장소 목록을 본다.
    - `git remote -v`
- __중앙 저장소에서 자기 브랜치 가져오기(갱신하기)__
  - `git fetch upstream {본인_아이디}`
    - Ex. git fetch upstream jungho
  - fetch 이후 `git branch -a` 로 확인하면 `remotes/upstream/baekjungho` 와 같은 브랜치가 생성된 것을 확인할 수 있다.
- __중앙 저장소와 동기화__
  - 현재 상태는 중앙 저장소 브랜치를 가져오기는 했지만 아직까지 로컬 저장소에 최신 버전의 코드가 반영된 것은 아니다.
  - `rebase` 명령을 실행해 중앙 저장소와 로컬 저장소의 브랜치를 동기화한다.
  - `git rebase upstream/본인_아이디`
    - Ex. `git rebase upstream/jungho`
  
```
PS C:\Users\qorwj\OneDrive\바탕 화면\Workspace\Nextstep\ATDD\atdd-subway-map> git branch -a
* baekjungho
step1
remotes/origin/baekjungho
remotes/upstream/baekjungho
```

## 충돌 해결

PR에서 충돌이 발생하는데 어떻게 해결하면 좋을까요?

> next-step 저장소와 rebase를 하지 않은 상태에서 브랜치를 생성했더니 충돌이 발생하고 있어요.
>
> 충돌을 해결하려면 어떻게 해야 하나요?

- git checkout 본인_아이디(예: git checkout jungho) 명령을 실행해 계정 브랜치로 이동한다.
- git reset --hard upstream/본인_아이디(예: git reset --hard upstream/jungho)
- git checkout 기능_브랜치(예: git checkout step2)
- git merge 본인_아이디(예: git merge jungho)
- 위 명령을 실행하면 충돌이 발생할 것이다. 충돌을 해결한 후 add, commit, push를 진행하면 PR 충돌이 해결되어 리뷰 요청을 할 수 있다.

## fetch / rebase

- fetch 는 원격 저장소에 있는 브랜치 내용을 Local Repository 로 가져오는 것을 의미한다.
- 원격 저장소에서 가져온  커밋들을 base 라고 하는데, 작업을 하고 PR 을 날린 상태에서 다른 사람의 작업물이 먼저 merge 되었다면 base 가 달라져서 이것을 맞추는 동기화 작업을 해야한다. 이것을 rebase 라고 한다.
- rebase : base 를 갱신하다
  - base : 각 저장소(원격 저장소, fork 한 내 저장소, clone 한 내 Local Repository)에 있는 `커밋 해시값들의 묶음` 이라고 생각하면 된다.
  - push 는 base 가 같아야만 할 수있다.

### 초기 상태

```java
// feature branch 는 release branch 가 commitX 일때 만들어진 브랜치
<release branch> <your feature branch>
commitX              commitX
commitY              commitA
commitZ              commitB
```

- 현재 release branch 는 다른 사람의 코드가 merge 된 상태여서 commitY, commitZ 가 추가되었다.
- 즉, `base 가 달라졌다.`
- 따라서 release branch 에 내 코드를 반영하기 위해서는 base 를 맞춰줘야 한다.

### STEP1

- 명령어를 입력하는 순간 Feature 브랜치의 루트 커밋(commitA)은 Release 브랜치의 가장 최근 커밋(commitZ)을 가리키게 된다.
- 이렇게 Feature 브랜치는 기존의 commitA 와 commitB 를 버리고(실제로 버리진않고 이걸 활용하긴 함) commitX, commitY, commitZ 를 가지게 된다.

```java
// STEP1
commitX
commitY
commitZ
```

### STEP2

- 이제 깃은 commitA 를 commitZ 뒤에 추가하려고 하는데, 이말은 즉 commitA 의 부모커밋이 commitX 에서 commitZ 로 변경이 된다는 말이다.
- 부모커밋이 변경됬으니 새로만들 commitA 의 해쉬값이 변경된다.
- 이 말은 즉, 새로운 커밋이 생성된다는 말이며, `커밋 메시지는 같다.` 부모의 해쉬값이 달라졌기때문에 당연히 commitA 의 해쉬값도 달라지게 된다.
- 만약 여기서 아무런 내용 충돌이 없다면 바로 commitZ 뒤에 commitA 를 추가하게 된다. 
- 이때 가장 중요한것은 지금의 commitA 의 해쉬값과 rebase 를 진행하기 전의 commitA 의 해쉬값은 다르다.
- 또한 위와 다르게 충돌이 발생한다면, 깃이 저희에게 이러한 내용을 알려줄것이고 저희는 이것을 직접 수동으로 충돌난 내용을 해결해야한다.
- 충돌을 해결하고 나면 아래의 명령어를 통해서 rebase 를 계속 해 나간다.
- 아래의 git add를 하게되면 staging 영역에 수정한 파일이 올라가게되고 git은 이 내용을 가지고 리베이스를 계속 진행하게 됩니다.
  - `git add fixedfile` : 충돌 난 파일을 add 해줌 
  - `git rebase --continue`

```java
// STEP2
commitX
commitY
commitZ
commitA
```

### STEP3

- STEP2 와 동일한 작업을 거친다.

```java
// STEP2
commitX
commitY
commitZ
commitA
commitB
```

커밋로그는 실제로 일자로 정렬이 된다.

![rebase](https://user-images.githubusercontent.com/47518272/152691012-5210125d-a0ec-4524-8201-a8668a8a5dbf.png)

이때 주의해서 볼것이 feature1 커밋(63e63b5)과 feature2 커밋(e81df68)인데, rebase 하기전의 이 커밋들의 값은 분명히 e261561 과 9dc8d0b 였다. 
하지만 이 커밋은 parent commit 이 initial commit 일때의 해쉬값이며 rebase 진행후에는 Release3 을 parent commit 으로 rebase 를 진행했으므로 commitA 의 해쉬값이 바뀌었고 commitA 의 해쉬값이 바뀌었으니 당연히 commitB 의 입장에선 부모(commitA)의 해쉬값이 바뀌었으니 자신의 해쉬값도 바뀌게 된다.

## merge vs rebase

- merge 를 할 경우에는 새로운 merge commmit 이 생기지만 rebase 는 merge commit 처럼 남는 커밋이 생기지 않는다.
- 따라서 커밋 히스토리를 깔끔하게 가져가려면 rebase 를 하는 것이 개인적으로 좋은 것 같다.

![mergecommit](https://user-images.githubusercontent.com/47518272/152691143-accb406e-cf99-4452-894d-62c3288b71bf.png)

## References

- [Git rebase](https://junwoo45.github.io/2019-10-23-rebase/)
- https://godtaehee.tistory.com/24
- https://github.com/next-step/nextstep-docs/blob/master/codereview/review-step3.md
