# RedirectAttributes

## addAttribute

Model 에 Key 와 Value 로 값을 담는다. 

```java
redirectAttributes.addAttribute("key", vo.getKey());
```

특징은 Key 와 Value 형태로 URL 에 `쿼리스트링(QueryString)` 처럼 붙어가기 때문에 addAttribute 를 사용하여 다른 핸들러 메서드로
redirect 하면 쿼리스트링에 있는 키와 매칭되는 커맨드 객체의 프로퍼티를 찾아서 값을 바인딩 시켜준다.

addAttribute 내부를 보면 아래와 같다.

```java
public class RedirectAttributesModelMap extends ModelMap implements RedirectAttributes {
  public RedirectAttributesModelMap addAttribute(String attributeName, Object attributeValue) {
      super.addAttribute(attributeName, this.formatValue(attributeValue));
      return this;
  }

  private String formatValue(Object value) {
      if (value == null) {
          return null;
      } else {
          return this.dataBinder != null ? (String)this.dataBinder.convertIfNecessary(value, String.class) : value.toString();
      }
  }
}
```    

`formatValue` 메서드를 보면 값을 스트링 형식으로 변환이 가능해야 한다. 불가능하면 에러가 발생한다. 즉, 객체자체로는 전달이 불가능하단 의미다.

## addFlashAttribute

addFlashAttribute 는 URL 뒤에 쿼리스트링으로 Key 와 Value 가 붙지 않는다. `일회성` 특징을 지니고 있어 리프레시 할 경우 데이터가 소멸된다.

