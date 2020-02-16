## HttpConnection ReadTimeout Exception

`ConnectTimeout` 은 연결이 아예 안되는 에러이며 `ReadTimeout` 은 연결은 성공 했지만 Timeout 안에 Data를 못 받으면 에러가 난다. 

Jsoup을 사용하는 경우 아래 처럼 중간에 timeout을 설정하면 된다.

```java
try {
    Document doc = Jsoup.connect(URL).timeout(10000).get();
    String html = doc.toString();
} catch (IOException e) {
    e.printStackTrace();
}
```
