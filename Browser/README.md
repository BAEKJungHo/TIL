# Web Browser

- 웹 브라우저(web browser)는 월드 와이드 웹에서 정보 자원을 검색하고, 표시하고, 횡단하기 위한 소프트웨어 애플리케이션이다.

## Web Browser Architectures

![browsermaincomponents](https://user-images.githubusercontent.com/47518272/157425772-0aa2be4a-c5c9-4c26-acb5-80d8acd7992d.png)

- __User Interface__
  - 주소 표시줄, 이전/다음/새로고침 버튼 등 `웹 페이지를 제외`하고 사용자와 상호작용하는 사용자 인터페이스
- __Rendering Engine__
  - HTML 과 CSS 를 파싱하여 요청한 웹 페이지를 표시하는 렌더링 엔진
  - Ex. Safari 는 Webkit, Firefox 는 Gecko, Chrome 은 Blink
- __Browser Engine__
  - 유저 인터페이스와 렌더링 엔진을 연결하는 브라우저 엔진
- __Networking__
  - 각종 네트워크 요청을 수행하는 네트워킹 파트
- __UI Backend__
  - 체크박스나 버튼과 같은 기본적인 위젯을 그려주는 UI 백엔드 파트
- __Data Persistence__
  - Local Storage 나 Cookie 와 같이 보조 기억장치에 데이터를 저장하는 파트
- __Javascript Interpreter__
  - Javascript 코드를 실행하는 인터프리터
  - Ex. Chrome V8 Engine

## Rendering Engine

- __렌더링 엔진의 목표__
  - HTML, CSS, JS, IMAGE 등 웹 페이지에 포함된 모든 요소들을 화면에 보여준다.
  - 업데이터가 필요할 떄, 효율적으로 렌더링을 할 수 있도록 자료 구조를 생성한다.
- __동작 과정__
  - ![render](https://user-images.githubusercontent.com/47518272/157427749-5cb38803-0b1d-47be-adb6-f28f6e98ef52.png)

### DOM Tree 와 CSSOM Tree 생성

![rendertoken](https://user-images.githubusercontent.com/47518272/157428134-a8e45442-c123-4983-bc5e-0e7ee3d5ef19.png)

- __DOM Tree__
  - ![domtree](https://user-images.githubusercontent.com/47518272/157428210-322b5d2e-e329-4fd1-b6a2-8845fdb046c6.png)
- __CSSOM Tree__
  - ![cssom](https://user-images.githubusercontent.com/47518272/157428531-b261af00-6bfd-48a5-ad99-4c27f321c56e.png)

### Render Tree 생성

![rendertree](https://user-images.githubusercontent.com/47518272/157428645-629be369-0f53-4772-9133-4ddd883052b5.png)

## Chrome Architecture

![chromearc](https://user-images.githubusercontent.com/47518272/157426023-9609378f-815a-4bc8-a6b4-22c0708eaf0a.png)

## References

- [체프의 브라우저 렌더링](https://www.youtube.com/watch?v=sJ14cWjrNis&t=94s)
