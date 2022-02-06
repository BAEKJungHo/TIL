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

## References

- [Git rebase](https://junwoo45.github.io/2019-10-23-rebase/)
- https://github.com/next-step/nextstep-docs/blob/master/codereview/review-step3.md
