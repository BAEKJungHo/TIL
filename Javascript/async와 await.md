## async 와 await

async 와 await 은 자바스크립트의 비동기 처리 패턴 중 가장 최근에 나온 문법입니다. 기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하고
개발자가 읽기 좋은 코드를 작성할 수 있게 도와줍니다.

> 읽기 좋은 코드 : 위에서부터 아래로 순서대로 처리되는 로직(코드)

```javascript
const user = fetchUser('domain.com/users/1');
if(user.id === 1) {
  console.log(user.name);
}
```

fetchUser() 메서드가 서버에서 사용자 정보를 가져오는 HTTP 통신 코드라고 가정하면, 위 코드는 `async & await` 문법이 적용된 형태라고 봐도됩니다.

### async & await 맛보기

```javascript
async function logName() {
  const user = await fetchUser('domain.com/users/1');
  if(user.id === 1) {
    console.log(user.name);
  }
}
```

위 코드가 바로 async await 코드입니다.

### async & await 문법

```javascript
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```

먼저 함수의 앞에 `async`라는 예약어를 붙입니다. 그리고 함수의 내부 로직 중 `HTTP 통신`을 하는 비동기 처리 코드 앞에 `await`을 붙입니다.

여기서 주의해야할 점음 비도기 처리 메서드가 꼭 `promise` 객체를 반환해야 await이 의도한 대로 동작한다는 사실입니다.

일반적으로 await의 대상이되는 비동기 처리 코드는 `Axios` 등 프로미스를 반환하는 API 호출함수 입니다.

```javascript
const result = (변수명) => {
    return new Promise((resolve, reject) => {
        $.getJSON(`${CONTEXT_PATH}/article/${변수명}.json`)
            .done(res => resolve(res))
            .fail(err => reject(err));
    });
};
```

### async & await 예외 처리

async & await 예외를 처리하는 방법은 바로 `try-catch`입니다. 프로미스에서 에러 처리를 위해 `.catch()`를 사용했던 것 처럼 async에서는 `catch {}`를
사용하면 됩니다.

```javascript
async function logTodoTitle() {
  try {
    const user = await fetchUser();
    if(user.id === 1) {
      const todo = await fetchTodo();
      console.log(todo.title);
    }
  } catch(error) {
    console.log(error);
  }
}
```



