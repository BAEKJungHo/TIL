# c:out

c:out을 사용하면 기본적으로 XSS 방지를 위해서 `""` 큰따옴표 같은것을  아스키 코드값으로 바꿔서 출력한다. 

따라서 console.log로 찍었을때 아스키코드가 안나오게 하기위해서는 `escapeXml="false"` 옵션을 주면 된다.

`escapeXml="false"` 를 쓰면 html 태그가 적용되서 나오고 사용하지않으면 html 태그 자체가 나온다.
