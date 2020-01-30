# JSONArray

java에서 객체(VO)에 담긴 프로퍼티 값들을 JSONObject에 키와 값 형태로 담고

jsonObject.put("key", value);

jsonObject를 JSONArray에 담아도 JSONArray에 담긴 값은 문자열로 취급된다.

jsonArray.put(jsonObject)

따라서 jsp에서 `${모델에 담긴 jsonArray 키값}"` 형태로 js로 뿌려도 

해당 js에서 받을때는 문자열로 받기 때문에 js로 넘길때 `JSON.parse('${모델에 담긴 jsonArray 키값}')` 형태로 보내주어야 JSON 형식으로 console에 출력된다.
