## deploy.sh

```shell
#!/bin/bash

## 변수 설정
EXECUTION_PATH=$(pwd)
SHELL_SCRIPT_PATH=$(dirname $0)
BRANCH=$1
PROFILE=$2
PROJECT_PATH=/home/ubuntu/nextstep/infra-subway-deploy

txtrst='\033[1;37m' # White
txtred='\033[1;31m' # Red
txtylw='\033[1;33m' # Yellow
txtpur='\033[1;35m' # Purple
txtgrn='\033[1;32m' # Green
txtgra='\033[1;30m' # Gray



function pull() {
  echo -e "${txtylw}=======================================${txtrst}"
  echo -e ">> Pull Request ${BRANCH} 🏃"
  git pull origin $BRANCH
  echo -e "${txtylw}=======================================${txtrst}"
}


function build() {
  echo -e "${txtylw}=======================================${txtrst}"
  echo -e ">> Gradle clean build 🏃"
  ./gradlew clean build
  echo -e "${txtylw}=======================================${txtrst}"
}


function kill_running_java_process() {
  echo -e "${txtylw}=======================================${txtrst}"
  CURRENT_PID=$(pgrep -f subway*.jar)

  if [[ -z "${CURRENT_PID}" ]]
          then
          echo ">> 실행 중인 애플리케이션이 없습니다."
  else
          echo -e ">> 실행 중인 애플리케이션의 PID(${CURRENT_PID})를 종료합니다. 🏃"
          echo "kill -15 ${CURRENT_PID}"
          sudo kill -15 ${CURRENT_PID}
          sleep 5
  fi
  echo -e "${txtylw}=======================================${txtrst}"
}


function deploy() {
  echo -e "${txtylw}=======================================${txtrst}"
  JAR_NAME=$(basename -- build/libs/*.jar)
  echo -e ">> Deploy ${JAR_NAME}🏃"

  nohup java -jar -Dspring.profiles.active=$PROFILE $JARFILE 1>log.txt 2>&1 &
  echo -e "${txtylw}=======================================${txtrst}"
}


## 조건 설정
if [[ $# -ne 2 ]]
then
  echo -e "${txtylw}=======================================${txtrst}"
  echo -e "${txtgrn}  << 스크립트 🧐 >>${txtrst}"
  echo -e ""
  echo -e "${txtgrn} $0 브랜치이름 ${txtred}{ prod | dev }"
  echo -e "${txtylw}=======================================${txtrst}"
  exit
fi

git fetch
master=$(git rev-parse ${BRANCH})
remote=$(git rev-parse origin/${BRANCH})

if [[ $master == $remote ]]; then
  echo -e "[$(date)] Nothing to do !!! 😫"
  exit 0
else
  echo -e "> 브랜치가 변경되었습니다."
  pull
  build
  kill_running_java_process
  deploy
fi

cd ${PROJECT_PATH}
```

## 분석

- __#!/bin/bash__
  - 쉘 스크립트를 만들 때, 첫 번째 줄에 사용하고 있는 쉘을 지정해야 한다.
  - 쉘 스크립트에서 첫 줄의 `#!` 를 제외하고는 # 으로 시작하는 기호는 모두 주석 처리된다. 
- __dirname $0__
  - dirname 은 리눅스에서 기본으로 제공하는 명령어 이며, dirname $0 은 쉘 스크립트에서 현재 실행되는 쉘 스크립트 파일의 실행 경로를 구할때 사용된다.
  - `$0` 은 실행한 쉘 스크립트의 경로가 지정된다. dirname 과 함께 사용하면 쉘 스크립트가 실행된 경로를 알아 낼 수 있다.
    - ```shell
      dirPath = `dirname $0`
      echo $dirPath
      cd $dirPath
      ```
