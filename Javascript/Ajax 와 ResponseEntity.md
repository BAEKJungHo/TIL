# Ajax 와 ResponseEntity

## Ajax Post Reqeust

```javascript
 let data = {
            "currency" : currency,
            "remittanceMoney" : remittanceMoney,
        }
$.ajax({
    type: "POST",
    url: `/exchangeRates`,
    data: JSON.stringify(data), // JSON 형식의 문자열로 변환
    headers: {'Content-Type': 'application/json'}, // json 데이터를 보낸다고 명시
    dataType: "json" // 응답으로 json 데이터를 받겠다는 의미
}).done(function (response) {
    let result = '수취금액은' + response.receipts + ' ' + response.currency + ' 입니다.';
    document.getElementById("receivedAmountDiv").textContent=result;
}).fail(function (error) {
    if(typeof error.responseJSON.message != 'undefined') {
        alert(error.responseJSON.message);
    } else {
        alert(error.responseJSON.errors[0].reason);
    }
});
```

POST 로 JSON 형식으로 데이터를 보냈기 때문에 @RequestBody 필요함

```java
 @PostMapping
public ResponseEntity<ExchangeRateCalculatingResponse> showReceivingMoney(
        @Valid @RequestBody ExchangeRateCalculatingRequest dto,
        BindingResult bindingResult
)
```

## Ajax Get Request : 데이터 2개 이상 보낼 때

```javascript
$.ajax({
    type: "GET",
    url: `/exchangeRates`,
    data: { currency : currency, remittanceMoney : remittanceMoney },
    dataType: "json"
}).done(function (response) {
    let result = '수취금액은' + response.receipts + ' ' + response.currency + ' 입니다.';
    document.getElementById("receivedAmountDiv").textContent=result;
}).fail(function (error) {
    if(typeof error.responseJSON.message != 'undefined') {
        alert(error.responseJSON.message);
    } else {
        alert(error.responseJSON.errors[0].reason);
    }
});
```

@RequestBody 필요 없음

```java
@GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
public ResponseEntity<ExchangeRateCalculatingResponse> showReceivingMoney(
        @Valid ExchangeRateCalculatingRequest dto,
        BindingResult bindingResult
) 
```
