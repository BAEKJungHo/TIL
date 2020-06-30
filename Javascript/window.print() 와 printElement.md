# 웹에서 프린트 구현하는 방법

- `window.print()`
  - 현재 페이지 전체에 대해서 프린트한다.
  - CSS 가 적용된다.
- `jQuery 의 printElement() function 사용하기`
    - 원하는 영역만 인쇄가 가능하다. 아래 에제에서 id 값을 이용하여 $("#printArea). ~~ 이런식으로 해당 영역만 인쇄가 가능하다.
    ```html
    <div id="printArea"></div>
    ```
    - 기본적으로 CSS 적용이 안되는데, 적용 가능하게 할 수도 있다
    
## 하나의 JSP 에서 여러개의 인쇄 목록을 뿌려서 인쇄하는 경우(forEach 사용)

예를들어 data 라는 리스트 안에 여러 값이 담겨있고, forEach 를 통해서, 인쇄 페이지의 발급 번호만 다르게 찍히게끔하여 인쇄하고 싶은 경우 어떻게 해야할까?

```html
<c:forEach items="${data}" var="result" varStatus="index">
  <div style="page-break-before : ${index.count eq 1 ? 'avoid' : 'always'}">
     <tr>
        <td><c:out value="${result.name}" /></td>
        ...
     </tr>
  </div>
</c:forEach>
```

위 처럼 사용가능하다. 즉, `page-break-before` 속성을 이용하는 방식이다. 위에서 `<div style="page-break-before : ${index.count eq 1 ? 'avoid' : 'always'}">` 이런식으로 준 이유는
첫 번째 페이지는 앞에서 분리를 하지 않기 위함이다. 즉, 공백 페이지를 없애기 위해서 이다.

```html
<div style="page-break-before: avoid;">1페이지 내용</div> // avoid 또는 style 제거
<div style="page-break-before: always;">2페이지 내용</div>
...
```

- auto : 자동계산
- inherit : 부모 요소의 값을 상속
- always : 앞에서 항상 분리
- avoid : 앞에서 분리 안함
- left : 앞에서 분리된 페이지가 왼쪽 면에 위치
- right : 앞에서 분리된 페이지가 오른쪽 면에 위치

