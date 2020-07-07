# Web Server와 WAS(Web Container).md

Web Server에는 무료 버전인 `Apache`와 Tmax 회사에서 만든 `WebToB`라는 웹 서버가 존재한다.(그 이외에도 더 있음)

Apache에서 제공하는 웹 컨테이너는(서블릿 컨테이너, WAS라고 불리기도함) `tomcat`이며, WebToB에서 제공하는 웹 컨테이너는 `jeus`이다.

보통 웹 서버에서는 여러개의 was를 띄울 수 있다. 

예를 들어 web server를 webtob를 사용하고 있다고 가정하고, webtob 아래에 jeus와 tomcat을 띄울 수 있다. 

우리가 도메인을 `http://www.example.kr/`을 입력했을때 `/`루트 인경우에는 jeus로 보내고 특정한 컨텍스트패스 `/dg`가 들어오면 tomcat으로 보내는 등
의 설정을 할 수가 있다.

위 예시처럼 webtob를 사용하고 있고, was는 jeus랑 tomcat 2개가 띄워져 있는 경우, webtob와 jeus는 호환이 가능해서 로그(mod_jk.log)를 볼 수 있지만,
tomcat에 대해서는 로그를 볼 수 없다. (tail -f ./catalina.out을 말하는게 아님) 만약 apache인경우에는 tomcat은 로그를 볼 수 있고 jeus는 볼 수 없다.

하나의 웹 서버에 여러개의 컨테이너를 띄울 수 있고, 하나의 컨테이너 안에 여러개의 프로젝트를 올릴 수 있는데 

- apache
  - jeus
    - project1
    - project2
    - project3
  - tomcat
    - project4
    
컨테이너를 증설하면 좋은점은, 하나의 컨테이너가 죽어도 해당 컨테이너에 있는 project만 죽는다. 예를들어 tomcat에 project 1to4까지 있는데
tomcat을 2개로 증설하면 project1to2 / 3to4 로 분리할 수 있어서 하나의 컨테이너를 내렸다 올릴때 1~2 혹은 3~4만 죽게된다. 단점은 메모리를 잡아먹는 양이 커지게 된다.

보통 web server는 정적 컨텐츠(html, image 등)을 처리할 수 있고 was는 정적 컨텐츠 + 동적컨텐츠(java파일 등)을 처리할 수 있다.

공공기관, 관공서같은 프로젝트를 진행하게 되면 보통 권고사항은 web server에는 정적컨텐츠 / was에는 동적컨텐츠를 넣는게 권고사항이지만 
부하가 적어서 was에 정적컨텐츠도 넣어도 상관없는경우에는 image와 같은 정적 컨텐츠 들도 was에 넣어서 처리하기도 한다.

## 컨텍스트 패스 붙이는 이유

도메인/cms
도메인/site 

이런식으로 컨텍스트 패스를 붙이는 이유는 다른 프로젝트에 대해서 같은 도메인을 사용하기 위함이다. 도메인이 같다는 것은 web server 가 같다는 것이다.

만약 관리자와 사용자가 web server 가 분리되어있으면 굳이 컨텍스트패스를 붙일 필요가 없다.

단, 기존에 사용하던 도메인에서  /abcd 이런식으로 해당 경로로 오는 요청의 경우 사용자 프로젝트로 쏴주는 경우에는 /abcd 를 사용자 프로젝트의 컨텍스트 패스에 설정해야 한다.


