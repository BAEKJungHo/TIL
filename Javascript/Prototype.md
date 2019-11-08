## Prototype

자바스크립트는 객체지향언어입니다. 하지만 자바스크립트는 클래스라는 개념이 존재하지 않는다. 최근 ECMA6 표준에서는 Class 문법이 추가 되었지만,
문법이 추가되었다는 뜻이지, 자바스크립트가 클래스 기반 언어로 바뀌었다는 것은 아닙니다.

클래스가 없으니 기본적으로 상속기능도 없습니다. 그래서 보통 프로토타입을 기반으로 상속을 흉내내도록 구현해 사용합니다.

## 프로토타입을 사용하는 이유

- 프로토타입 사용 전 

```javscript
function Person() {
  this.eyes = 2;
  this.nose = 1;
}

const kim = new Person();
const baek = new Person();

console.log(kim.eyes); // 2
console.log(kim.nose); // 1
console.log(baek.eyes); // 2
console.log(baek.nose); // 1
```

Person이라는 함수를 생성하여 내부에 eyes와 nose라는 객체를 생성하고 new 연산자로 생성자를 호출하여 사용하게 되면 메모리를 2배나 더 잡아먹는다.

즉, 메모리에는 eyes와 nose객체가 2개씩 총 4개가 할당된다.

이러한 문제를 프로토타입으로 해결할 수 있다.

```javascript
function Person() {}

Person.prototye.eyes = 2;
Person.prototye.nose = 1;

const kim = new Person();
const baek = new Person();

console.log(kim.eyes); // 2
console.log(kim.nose); // 1
console.log(baek.eyes); // 2
console.log(baek.nose); // 1
```

메모리에 객체가 1개씩 총 2개가 할당된다.

> `프로토타입`은 빈 공간에 객체를 넣어두고 공유하여 사용할 수 있도록 한다.

## Prototype : Prototype Link + Prototype Object

자바스크립트에는 Prototype Link와 Prototype Object가 존재하고 이 둘을 통틀어 Prototype이라고 부른다. 프로토타입을 좀 안다는 것은
이 둘을 완벽히 이해하고 가지고 놀 수준이 되었다는 뜻이다.

## Prototype Object

객체는 언제나 `함수(Function)`로 생성된다.

```javscript
function Person() {} // 함수

const personObject = new Person(); // 함수로 객체를 생성
```

personObject 객체는 Person이라는 함수로 생성된 객체이다. 이렇듯 언제나 객체는 함수에서 시작된다. 

```javascript
const obj = {}; // const obj = new Object();
```

Object와 마찬가지로 `function, array` 모두 함수로 정의되어있다. 

여기서 중요한 포인트는 `함수가 정의될 때 2가지 일이 동시에 이루어집니다.`

### 1. 해당 함수에 Constructor(생성자) 자격 부여 

Constructor 자격이 부여되면 `new`를 통해 객체를 만들어 낼 수 있게 된다. 이것이 `함수만 new 키워드를 사용할 수 있는 이유`이다.

만약 Constructor 자격이 없으면, 개발자도구 콘솔창에서 아래와 같은 오류가 발생한다.

`Uncaught TypeError : obj is not a constructor(...)`

### 2. 해당 함수의 Prototype Object 생성 및 

함수를 정의하면 `Prototype Object`도 같이 생긴다. 예를들어 `function Perosn() {}` 이라는 함수를 정의하면 생성자 자격이 부여됨과 동시에
`Person Prototype Object` 가 생긴다.

function Perosn() {} 내부에는 `prototpye`이 존재하고 이 prototpye이 `Person Prototpye Ojbect`를 가리킨다. `Person Prototype Object` 내부에는
`constructor`와 `__proto__`가 존재한다.

- function Perosn() {}
  - `prototpye`
- Person Prototype Oject
  - `constructor`
  - `__proto__` = Prototype Link

> 여기서 주의해야할 점!! `prototpye`은 함수에서만 존재하는 녀석이고 `__proto__`는 모든 속성에 다 존재하는 녀석이다.

따라서 prototype 속성을 이용해 Prototype Object에 접근할 수 있다.

`__proto__`는 객체가 생성될 때 조상이었던 함수의 `Prototype Object`를 가리킨다. kim과 baek 객체는 Person 함수로 부터 생성되었으니
Person 함수의 `Person Object`를 가리키는 것이다.

```javascript
kim.__proto__
```

위 처럼 접근해서 까보면 kim 객체 내부에 eyes 객체와 nose 객체가 존재하는 것을 볼 수 있습니다.

이렇게 __proto__속성을 통해 상위 프로토타입과 연결되어있는 형태를 프로토타입 체인(Chain)이라고 한다.

아래 예제를 통해 한번 확인해보겠습니다.

```javascript
function Ultra() {]
Ultra.prototype.ultraProp = true;

function Super() {}
Super.prototype = new Ultra();

function Sub() {}
Sub.prototpye = new Super();

const obj = new Sub();

console.log(obj.ultraProp); // true
```

생성자 Sub를 통해서 만들어진 객체 obj가 Ultra의 ultraProp 프로퍼티에 접근이 가능한 것은 `prototype chain`으로 Sub와 Ultra 객체가 연결되어 있기 때문이다. 내부적으로는 아래와 같이 동작한다.

1. 객체 obj가 ultraProp을 찾는다.
2. 없다면 Sub.prototpye.ultraProp을 찾는다.
3. 없다면 Super.prototpye.ultraProp을 찾는다.
4. 없다면 Ultra.prototpye.ultraProp을 찾는다.

## Reference

> https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67
>
> https://opentutorials.org/course/743/6573
