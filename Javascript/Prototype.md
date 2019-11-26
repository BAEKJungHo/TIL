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

### 2. 해당 함수의 Prototype Object 생성

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

## OREILLY PROTOTYPE

클래스의 인스턴스에서 사용할 수 있는 메서드라고 하면 그건 프로토타입 메서드를 말하는 겁니다. 프로토타입 메서드는 `Car.prototype.shift` 처럼
표기할 때가 많습니다. Array의 forEach를 Array.prototype.forEach라고 쓰는것과 마찬가지 입니다. 자바스크립트가 프로토타입 체인을 통해 어떻게
`동적 디스패치(dynamic dispatch)`를 구현하는지 알아봅시다. 

> 최근에는 프로토타입 메서드를 #으로 표시하는 표기법이 널리 쓰입니다. 예를들어 Car.prototpye.shift를 Car#shit로 쓰는겁니다.

모든함수에는 prototype이라는 특별한 프로퍼티가 있습니다. 일반적인 함수에서는 프로토타입을 사용할 일이 없지만, 객체 생성자로 동작하는 함수에서는
프로토타입이 중요합니다. 

함수의 `prototype` 프로퍼티가 중요해지는 시점은 `new` 키워드로 새 인스턴스를 만들었을 때입니다. new 키워드로 만든 새 객체는 생성자의 prototype 프로퍼티에 접근할 수 있습니다. 객체 인스턴스는 생성자의 prototype 프로퍼티를 `__proto__` 프로퍼티에 저장합니다.

> `__proto__` 프로퍼티는 자바스크립트의 내부 동작 방식에 영향을 미칩니다. 밑줄 두 개로 둘러싼 프로퍼티는 모두 그렇습니다. 이런 프로퍼티를
직접 수정하는것은 정말로 위험합니다. 자바스크립트를 충분히 이해하기 전에는 살펴보기만하고 손대지는 말길 추천합니다.

프로토타입에서 중요한 것은 `동적 디스패치`라는 메커니즘입니다. 여기서 디스패치는 메서드 호출과 같은 의미입니다. 객체의 프로퍼티나 메서드에 접근하려
할 때 그런 프로퍼티나 메서드가 존재하지 않으면 자바스크립트는 객체의 프로토타입에서 해당 프로퍼티나 메서드를 찾습니다. 클래스의
인스턴스는 모두 같은 프로토타입을 공유하므로 프로토타입에 프로퍼티나 메서드가 있다면 해당 클래스의 인스턴스는 모두 그 프로퍼티나 메서드에 접근할 수 있습니다.

```javascript
const car1 = new Car();
const car2 = new Car();
car1.shift === Car.prototpye.shift;
```

## hasOwnProperty

```javascript
obj.hasOwnProperty(x); 
```

obj에 프로퍼티 x가 있다면 true를 반환하며, 프로퍼티 x가 obj에 정의되지 않았거나 프로토타입 체인에만 정의되었다면 false를 반환합니다.

ES6 클래스를 설계 의도대로 사용한다면 데이터 프로퍼티는 항상 프로토타입 체인이 아니라 인스터늣에 정의해야합니다. 하지만 프로퍼티를 프로토타입에
정의하지 못하도록 강제하는 장치는 없으므로 확실히 확인하려면 항상 hasOwnProperty를 사용하는 편이 좋습니다.

```javascript
class Super {
  constructor() {
    this.name = 'Super';
    this.isSuper = true;
  }
}

// 유효하지만 권장하지는 않습니다.
Super.prototype.sneaky = 'not recommended!';

class Sub extends Super {
  constructor() {
    super();
    this.name = 'Sub';
    this.isSub = true';
  }
}

const obj = new Sub();

for(let p in obj) {
  console.log(`${p}: ${obj[p]}`+ (obj.hasOwnProperty(p) ? '' : '(inherited)'));
}
```

## Reference

> https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67
>
> https://opentutorials.org/course/743/6573
