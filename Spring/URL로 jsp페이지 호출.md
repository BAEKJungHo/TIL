# URL로 jsp 페이지 호출


jsp 파일이 WEB-INF 아래에 있으면 보안상 문제 때문에 호출되지 않는다. `ex) localhost:8080/contextpath/WEB-INF/폴더명/jsp파일명`

따라서 jsp 파일을 webapp 바로 밑에다 두어야 한다. WEB-INF 밑에 있으면 Tiles 태워서 가능
