# Mixed Content

- Error 내용
  - ... was loaded over HTTPS, but requested an insecure script ... This request has benn blocked; the content must be served over HTTPS.
  
> 혼합 콘텐츠(Mixed Content)는 HTTPS 를 통해 접속한 사이트에서 HTTP 통신을 통해 스크립트나 CSS, 이미지, 동영상 자원등을 요청하는 것

- 혼합 콘텐츠의 종류
  - 혼합 콘텐츠는 두 가지로 분류되는데, 수동적 혼합 콘텐츠와, 능동적 혼합 콘텐츠로 분류됩니다.
  - 수동적(Passive) 혼합 콘텐츠 
    - 이미지(사진, 그림 등)나 비디오 오디오같은 콘텐츠를 HTTP 로 요청하는 것으로, 공격자가 중간에서 가로채어 파일을 수정하더라도 사이트 전체에 상호작용이 일어나는 콘텐츠가 아니므로 능동적 혼합 콘텐츠보다 영향이 덜 합니다. 그러나 광고 이미지나 비디오, 음란물로 변경될 수 있는 위험이 있습니다.
  - 능동적(Active) 혼합 콘텐츠 
    - 브라우저가 실행하게 되는 스크립트, CSS, iframe, 플래시 등의 콘텐츠를 HTTP 로 요청하는 것으로 공격자가 중간에 가로채어 코드를 수정하고 이를 실행하게 되면 사이트 전체에 영향이 가해지므로 수동적 혼합 콘텐츠 보다 영향이 큰 혼합 콘텐츠입니다. 스크립트 등을 악의적으로 변경하여 이상한 URL로 리다이렉트 되거나, 사용자 정보를 빼내는 스크립트로 대체될 수 있는 위협이 있습니다.

- 크롬에서 혼합 콘텐츠 분석하기
  - 크롬의 개발자 모드(F12키로 접근)를 이용하면 혼합 콘텐츠를 분석할 수 있습니다. 즉 HTTP 로 수신하는 자원들이죠. 크롬에서는 수동적 혼합 콘텐츠는 노란색으로 능동적 혼합 콘텐츠는 빨간색으로 보여줍니다.
  
## 문제해결방법

1. 스크립트나 정적자원 불러오는 스킴을 https 로 바꿔서 제대로나오는지 확인한다.
2. 만약 제대로 나오지 않는다면, 스크립트의 경우 http 로 불러오는 스크립트 url 을 웹에서 입력 후 나오는 소스를 긁어서 따로 파일로 만들어서 자체적으로 관리한다.

## References.

> https://dololak.tistory.com/611
