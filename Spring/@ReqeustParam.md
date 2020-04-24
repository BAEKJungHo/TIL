## @RequestParam

/hello/1?name=jungho&age=27

처럼 되어있을때 name을 받고 싶은경우

id 값 1은

@GetMapping("/hello/{id}") 처럼 사용하여 @PathVariable 로 받을 수 있다

@RequestParam String name 으로 할 수 있다.

만약 name 과 age 둘다 받고 싶은 경우 컴포짓 객체 방식으로 @ModealAttribute User user 로 받을 수 있다.


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

