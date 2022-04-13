# CORS 로컬에서 테스트

- 인텔리제이로 로컬 띄우고
- Terminal 
  - ```
      curl \
    --verbose \
    --request OPTIONS \
    'http://localhost/sales' \
    --header 'Origin: http://sales-front.popit.kr' \
    --header 'Access-Control-Request-Headers: Origin, Accept, Content-Type' \
    --header 'Access-Control-Request-Method: GET'
    ```

Test 하고자 하는 URL 을 `http://localhost/sales` 부분에 적는다.

## References

- https://www.popit.kr/curl-%EB%AA%85%EB%A0%B9%EC%96%B4%EB%A1%9C-%ED%95%98%EB%8A%94-%EC%B4%88%EA%B0%84%EB%8B%A8-cors-%ED%85%8C%EC%8A%A4%ED%8A%B8/
