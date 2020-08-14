# Implicit Object

JSP 의 Implicit Object 는 내포된 객체라는 의미로 톰캣(WAS) 가 생성해줘서, 따로 page import 없이 사용할 수 있는 객체를 의미한다. 예를 들면 request 나 response 같은 애들이다.

> https://www.javatpoint.com/jsp-implicit-objects

## JSP 동작 과정

JSP 는 내부적으로 Servlet 으로 자동 변환 된다.

JSP가 실행되면 WAS는 내부적으로 JSP 파일을 Java Servlet(.java)으로 변환한다.

WAS는 이 변환한 Servlet을 동작하여 필요한 기능을 수행한다.

1) WAS는 사용자 요청에 맞는 적절한 Servlet 파일을 컴파일(.class 파일 생성)한다.
2) .class 파일을 메모리에 올려 Servlet 객체를 만든다.
3) 메모리에 로드될 때 Servlet 객체를 초기화하는 init() 메서드가 실행된다.
4) WAS는 Request가 올 때마다 thread를 생성하여 처리한다.
5) 각 thread는 Servlet의 단일 객체에 대한 service() 메서드를 실행한다.
6) service() 메서드는 요청에 맞는 적절한 메서드(doGet, doPost 등)를 호출한다.

수행 완료 후 생성된 데이터를 웹 페이지와 함께 클라이언트로 응답한다.

> https://gmlwjd9405.github.io/2018/11/03/jsp.html
