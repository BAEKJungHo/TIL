# 자바스크립트 Promise 쉽게 이해하기

## Promise 란?

> A promise is an object that may produce a single value some time in the future

프로미스는 자바스크립트 비동기 처리에 사용되는 객체입니다. 여기서 자바스크립트의 비동기 처리란 `특정 코드의 실행이 완료될 때 까지 기다리지 않고 다음 코드를 먼저 수행하는
자바스크립트의 특성`을 의미합니다.

## Promise가 왜 필요한가요?

프로미스는 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용합니다. 일반적으로 웹 애플리케이션을 구현할 때 서버에서 데이터를 요청하고 받아오기 위해 
아래와 같은 API를 사용합니다.

```javascript
$.get('url 주소/products/1', function(response) {
  // ...
});
```

위 API가 실행되면 서버에다가 `데이터 하나 보내주세요` 라는 요청을 보냅니다. 그런데 여기서 데이터를 받아오기도 전에 마치 데이터를 다 받아온 것 마냥 화면에
데이터를 표시하려고 하면 오류가 발생하거나 빈 화면이 뜹니다. 이와 같은 문제점을 해결하기 위한 방법 중 하나가 프로미스 입니다.

## 프로미스 코드 - 기초

- 콜백함수를 이용한 비동기 처리 코드

```javascript
function getData(callbackFunc) {
  $.get('url 주소/products/1', function(response) {
    callbackFunc(response);
  });
}

getData(function(tableData) {
  console.log(tableData);
});
```

위 코드에 프로미스를 적용하면 아래와 같은 코드가 됩니다.

```javascript
function getData(callback) {
  // new Promise()
  return new Promise(function (resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      // 데이터를 받으면 resolve() 호출
      resolve(response);
    });
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function (tableData) {
  // resolve()의 결과값이 여기로 전달됨
  console.log(tableData);
});
```

## 프로미스의 3가지 상태(states)

프로미스를 사용할 때 알아야 하는 가장 기본적인 개념이 바로 프로미스의 상태(states)입니다. 여기서 말하는 상태란 프로미스의 처리 과정을 의미합니다.
`new Promise()`로 프로미스를 생성하고 종료될 때까지 3가지 상태를 갖습니다.

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
  - resolve, reject에 접근할 수 있다.
- Fulfilled(이행 = 완료) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
  - 내부에서 resolve()를 실행하면 이행상태가 된다.
  - then()을 통해서 처리 결과 값을 받을 수 있다.
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태
