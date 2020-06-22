# JSONArray

java 에서 객체에 담긴 프로퍼티 값들을 JSONObject 에 키와 값 형태로 담고 

`jsonObject.put("key", value);`

jsonObject 를 JSONArray 에 담아도 JSONArray 에 담긴 값은 문자열로 취급된다.

`jsonArray.put(jsonObject)`

따라서 jsp 에서 `${모델에 담긴 jsonArray 키값}"` 형태로 js 로 뿌려도 

해당 js 에서 받을때는 문자열로 받기 때문에 js로 넘길때 `JSON.parse('${모델에 담긴 jsonArray 키값}')` 형태로 보내주어야 JSON 형식으로 console 에 출력된다.

```html
uploadSave.save(JSON.parse('${largeFiles}'));
```
