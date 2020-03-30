# HandlerInterceptor

- HandlerInterceptor
  - 핸들러 매핑에 설정할 수 있는 인터셉터
  - 핸들러를 실행하기 전, 후 (아직 렌더링 전) 그리고 완료(랜더링까지 끝난 후) 시점에 부가 작업을 하고 싶은 경우에 사용할 수 있다.
  - 여러 핸들러에서 반복적으로 사용하는 코드를 줄이고 싶을 때 사용할 수 있다.
    - 로깅, 인증체크, Locale 변경 등
    
## boolean preHandle(request, response, handler) : 전처리

- 핸들러 실행하기 전에 호출 됨
- 핸들러에 대한 정보를 사용할 수 있기 대문에 서블릿 필터에 비해, 보다 세밀한 로직을 구현할 수 있다.
- 리턴값으로 계속 다음 인터셉터 또는 핸들러로 요청, 응답을 전달할지(true) 응답 처리가 이곳에서 끝났는지(false) 알린다.

## void postHandle(request, response, modelAndView) : 후처리 (뷰 랜더링 이전)

- 핸들러 실행이 끝나고 아직 뷰를 랜더링 하기 이전에 호출 됨
- 뷰에 전달할 추가적이거나 여러 핸들러에 공통적인 모델 정보를 담는데 사용할 수 도 있다.
- 이 메서드는 인터셉터 역순으로 호출된다.
- 비동기적인 요청 처리 시에는 호출되지 않는다.

## void afterCompletion(request, response, handler, ex) : 후처리 (뷰 랜더링 이후)

- 요청 처리가 완전히 끝난 뒤(뷰 랜더링까지 끝난 뒤) 호출 됨
- preHnadler 에서 true 를 리턴한 경우에만 호출 됨
- 이 메서드는 인터셉터 역순으로 호출된다.
- 비동기적인 요청 처리 시에는 호출되지 않는다.

## 핸들러 인터셉터 VS 서블릿 필터

> 서블릿 필터 보다 구체적인 처리가 가능하며(HandlerInterceptor 의 테크닉이 더 구체적이다), 일반적인 용도의 기능을 구현할 때 사용하는게 좋다.
만약, 스프링과 관련되어 있지 않은 로직이면 서블릿 필터로 구현하는게 낫다. 예를들어 XSS ATTACK 과 같은 처리를 하기 위해서는 서블릿 필터로 구현되는게 맞다. (LUCY XSS 처럼 서블릿 필터가 있다.)

## Example

Interceptor 는 WebConfig 에서 설정 할 수 있다. 

```java
@Configuration
@ComponentScan(basePackages = "com.baek")
@ServletComponentScan(basePackages = "com.baek.core")
public class WebConfig extends WebMvcConfigurerAdapter {
  
  /**
  * postHandle 과 afterCompletion 은 역순으로 호출된다.
  * 명시적으로 순서를 주고 싶으면 order 를 설정해야 한다.
  * order 값은 낮을 수록 우선순위가 높다.
  */
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new GreetingInterceptor()).order(0);
    registry.addInterceptor(new AnotherInterceptor()).order(-1);
  }

}
```

Interceptor 를 원하는 특정 패턴에 적용 및 배제 할 수 도 있다.

```java
@Configuration
@ComponentScan(basePackages = "com.baek")
@ServletComponentScan(basePackages = "com.baek.core")
public class WebConfig extends WebMvcConfigurerAdapter {
  
  /**
  * 원하는 특정 패턴 적용 및, 특정 패턴 배제
  */
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new GreetingInterceptor()).order(0);
    registry.addInterceptor(new AnotherInterceptor())
      .addPathPatterns("/hi");
      .excludePathPatterns("/api/**");
      .order(-1);
  }

}
```
