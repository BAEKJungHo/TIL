# us7ascii 한글 인코딩 문제

db charset 이 us7ascii 인 경우 한글에서 지원하지 않는 문자가 있다.

> ex) `퉥,뷁 등`

해당 charset 에서 대응되는 글자가 없는 경우 `java char로 0xfffd` (물음표, ?)로 변환된다.

예를들어 고구마퉥퉥 이라고 vo 에 담아서 해당 값을 아래처럼 변환하면 고구마?? 라고 나온다.

```java
encodedStr = new String(("고구마퉥퉥").getBytes("EUC_KR"), "8859_1");
```

이러한 문제를 해결하기 위해서 `암호화` 하여 DB 에 저장하고, 꺼낼 땐 `복호화`해서 사용하는 방법이 있고

두 번째는 자바에서 처리할 수 있다.

예를 들어 닉네임 검증을 해야하는 경우 다음과 같이 할 수 있다.

```java
// decodedStr 과 파라미터가 동일하지 않으면, 해당 charset 에서 지원하지 않는 문자이다.
public static boolean canUseNickname(String str) {
  String encodedStr = null;
  String decodedStr = null;
  try {
    encodedStr = new String((str).getBytes("EUC_KR"), "8859_1");
    decodedStr = new String((encodedStr).getBytes("8859_1"), "EUC_KR");
  } catch (UnsupportedEncodingException e) {
    e.printStackTrace();
  }

  return str.equals(decodedStr);
}
```

## 참고

> [인코딩 8859_1 의 비밀](https://blog.naver.com/anabaral/130043451093)
