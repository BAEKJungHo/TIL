# ajax 사용시 always complete, done

```javascript
$.ajaxPost = (url, data) => {
   return $.ajax({
        url: url,
        data: data,
        method: 'POST',
        dataType: 'json',
        contentType:'application/json'
    });
};

...
$.ajaxPost(`${CONTEXT_PATH}/xxx/api/remove`, JSON.stringify(params))
  .complete(data => alert(data.responseText), removeLinkUrlTag(obj));
```

.done 

.always

이렇게 사용하는 경우는 always 부터 작동한다. done 보다 먼저 동작

성공한 경우에만 동작하게 하고싶으면 complete 사용
