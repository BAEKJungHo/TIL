# BindingResult

```java
public String create(@Validated @ModelAttribute Event event, BindingResult bindingResult) {
  if(bindingResult.hasErrors()) {
    bindingResult.getAllErrors().forEach(error -> {
      System.out.println(error);
    });
  }

  return "redirect:/list";
}
```
