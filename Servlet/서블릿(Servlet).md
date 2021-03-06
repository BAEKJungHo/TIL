# Servlet Application

- Servlet
  - 자바 엔터프라이즈 에디션은 웹 애플리케이션 개발용 스펙과 API 제공
  - 요청당 스레드 사용(한 프로세스 내의 자원을 공유하는 스레드)
  - 가장 중요한 클래스 중 하나가 HttpServlet

- CGI(Common Gateway Interface)
  - 서블릿 등장 이전에 사용하던 기술
  - 요청당 프로세스 만들어 사용
  
- 서블릿의 장점(CGI에 비해)
  - 빠르다
  - 플랫폼 독립적
  - 보안
  - 이식성
  
- 서블릿 엔진 또는 서블릿 컨테이너(톰캣, 제티, 언더토우 ...)
  - 세션관리
  - 네트워크 서비스
  - MIME 기반 메시지 인코딩 디코딩
  - 서블릿 생명주기 관리
  
## 서블릿 생명주기

- 서블릿 컨테이너가 서블릿 인스턴스의 `init()` 메서드를 호출하여 초기화한다.
  - 최초 요청시 한번만 초기화하고, 그 뒤로는 이 과정을 생략한다.
- 서블릿이 초기화된 다음부터 클라이언트의 요청을 처리할 수 있다. __각 요청은 별도의 스레드로 처리하고 이때 서블릿 인스턴스의 `service()` 메서드를 호출한다.__  (스프링에서 톰캣을 사용하는 경우도 마찬가지로 톰캣이 서블릿 컨테이너이므로 클라이언트 하나의 요청당 하나의 스레드를 생성해서 처리한다.
  - 이 안에서 HTTP 요청을 받고 클라이언트로 보낼 HTTP 응답을 만든다. 
  - service()는 보통 HTTP Method에 따라 `doGet()`, `doPost()` 등으로 위임 처리한다.
  - 따라서 보통 doGet() 또는 doPost()를 구현한다.
- 서블릿 컨테이너 판단에 따라 서블릿을 메모리에서 내려야할 시점에 `destroy()`를 호출한다. 

> 서블릿을 동작하게 하는건 우리가 하는게 아니라 서블릿 컨테이너가 한다.

## Example

```java
public class HelloServlet extends HttpServlet {
  @Override
  public void init() throws ServletExcetion {
    System.out.println("init");
  }
  @Override
  public void doGet(HttpServletReqeust req, HttpServletResponse res) throws ServletExcetion {
    System.out.println("doGet");
  }
  @Override
  public void destory() {
    System.out.println("destroy");
  }
}
```
