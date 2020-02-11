## redirect로 파라미터 값 넘기기

```java
redirect 하는 메서드(User user) {
      redirectAttributes.addAttribute("userMenuSeq", user.getUserMenuSeq());
      redirectAttributes.addFlashAttribute("message", Message.UPDATED.getMsg());
      return REDIRECT;
}


리다이렉트 파라미터 받아야 하는 메서드(@RequestParam("키") String userMenuSeq) {

}
```

> 즉, redirectAttributes.addAttribute("키", "값") 로 넘겨주고 @ReqesutParam을 이용해서 받으면 된다.

