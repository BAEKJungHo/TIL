# Coding Style

## Example 1.

로그인 후 자신이 작성한 게시글 or 댓글에 대한 수정이나, 삭제를 진행해야하는 경우 서버단에서는 로그인한 ID 와 게시물(댓글) 키값을 이용해서 조회 한 결과가 존재하는지 검증하고
수정 or 삭제를 진행해야한다.

이러한 경우에 따로 count 하는 쿼리를 만들기 보단 기존에 존재하는 find 쿼리가 있으면 다음과 같이 하면된다.

- 서비스

```java
public void updateArticle(ArticleVo articleVo) {
  ArticleVo article = Optional.ofNullable(articleRepository.findArticle(articleVo))
        .orElseThrow(() -> new NotFoundDataException(Message.NOT_FOUND_DATA)));
  article.update(articleVo);
  articleRepository.updateArticle(article);
}
```

- VO 

```java
public void update(ArticleVo articleVo) {
  this.regId = articleVo.getRegId();
  // 생략
}
```

즉, 데이터 조회한 결과를 이용해서 결과가 존재하는 경우 조회한 결과와 파라미터로 받은 vo 에 들어있는 update 할 속성을 설정한 다음 수정한다.
