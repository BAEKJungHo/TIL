# GET 방식에 대해서

form 안에 input 데이터 들이 많이 있는 경우 특히  textArea 내부 데이터 값의 크기가 상당히 큰경우

GET방식으로 넘기게 되면 url에 쿼리스트링(querystring) 으로 붙어 날아간다. 

따라서 input 이 담기지 않은 폼을하나 만들어서 보내는게 낫다.

- input 담긴 폼 이름 : xxx
- input이 없는 폼 이름 : yyy

```javascript
this.yyy.action = ~~
this.yyy.method = GET
this.yyy.submit()
```

> 즉, GET 방식은 form 내부의 input 에 있는 데이터들을 쿼리스트링으로 키와 밸류 형식으로 보낸다.
