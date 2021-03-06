# JavaScript

## 자바스크립트 버전

- **ECMAScript**는 자바스크립트의 핵심 문법(core syntax)을 규정한다.
- 각 **브라우저** 제조사는 ECMAScript를 준수하여 브라우저에 내장되는 **자바스크립트 엔진**을 구현한다.
- 즉, 자바스크립트 버전은 ECMAScript(ES)의 버전에 따라서 결정된다. 이를 자바스크립트 실행 엔진이 반영한다. (브라우저마다 자바스크립트 실행 엔진이 있다)

## Node.js

- 브라우저에서만 동작하던 자바스크립트를 **브라우저 이외의 환경**에서 동작시킬 수 있는 자바스크립트 실행 환경(런타임)이다.

## 연산자 - 비교 연산자

- 비교는 `==` 보다는 `===`를 사용한다.
- `==` 으로 인한 다양한 오류 상황이 있다.
- 암묵적으로 타입을 바꿔서 비교한다.

```javascript
0 == false; // true
"" == false; // true
0 == "0"; // true
null == undefined; // true
null == false; // false
```

## 프로토타입

- 자바스크립트는 프로토타입 기반 언어
- **부모 객체를 Prototype(프로토타입)객체 또는 Prototype(프로토타입)이라 한다.**
- 프로토타입 객체(부모객체)는 생성자 함수에 의해 생성된 각각의 객체에 공유 프로퍼티를 제공하기 위해 사용한다.
- 자바스크립트의 모든 객체는 `__proto__`를 가진다.
- `__proto__`는 자신의 부모 객체(프로토타입 객체)를 가리킨다.

```javascript
var student = {
  name: "Lee",
  score: 99,
};

console.log(student.__proto__ === Object.prototype); // true
```

- 함수도 객체이므로 `__proto__`를 가진다. 그런데 함수 객체는 일반 객체와는 달리 prototype 프로퍼티(부모 역할을 할 객체)도 소유하게 된다.
- **즉, 함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성된다.**
- 객체는 언제나 함수(Function)으로 부터 생성된다.<br> ex) `var personObject = new Person();`
- 프로토타입 객체(부모 객체)는 constructor 프로퍼티를 갖는다.
- constructor 프로퍼티는 생성자 함수 자체를 가리킨다.

```javascript
function Person(name) {
  this.name = name;
}

var foo = new Person("Lee");

console.log(Person.prototype.constructor === Person);
```

### Pototype chain

- 자바스크립트는 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 프로퍼티 또는 메소드가 없다면 `__proto__`가 가리키는 링크를 따라 자신의 부모 역할을 하는 프로토탐입 객체의 프로퍼티나 메소드를 차례대로 검색한다.
- 이것을 프로토타입 체인이라 한다.

#### 객체의 프로토타입 체인

```javascript
var person = {
  name: "Lee",
};

console.log(person.__proto__); // object prototype
console.log(Object.prototype.constructor === Object); //true
console.log(Object.__proto__ === Function.prototype); // true
console.log(Function.prototype.__proto__ === Object.prototype); // true
```

#### 생성자 함수로 생성된 객체의 프로토타입 체인

```javascript
function Person(name) {
  this.name = name;
}

var foo = new Person("Lee");

console.log(foo.__proto__ === Person.prototype);
// true
console.log(Person.prototype.__proto__ === Object.prototype);
console.log(Person.prototype.constructor === Person);
console.log(Person.__proto__ === Function.prototype);
console.log(Function.prototype.__proto__ === Object.prototype);
```

- 결국은 모든 객체의 부모 객체인 Object.prototype 객체에서 프로토타입 체인이 끝나기 때문에
- Object.prototype 객체를 프로토타입 체인의 종점이라 한다.

### Object.create(prototype, 속성)

- 객체를 생성하는 방법 중 하나
- prototype을 설정할 수 있다.

```javascript
var obj = {}; // Object.create(Object.prototype); 과 같음
```

## for-in문과 for-of문

- for-in문은 객체의 프로퍼티를 순회하기 위해 사용하고
- for-of문은 배열의 요소를 순회하기 위해 사용한다.

## 클래스

```javascript
class Foo {}
const foo = new Foo();
```

- Foo는 사실 생성자 함수(constructor)이다.
- constructor는 생략할 수 있다.
- constructor는 생략하면 클래스에 constructor() {}를 포함한 것과 동일하게 동작한다.
- 즉, 빈 객체를 생성한다.
- 클래스 필드는 언제나 public 이다.

### getter

```javascript
class Foo {
  constructor(arr = []) {
    this._arr = arr;
  }

  // getter: get 키워드 뒤에 오는 메소드 이름 firstElem은 클래스 필드 이름처럼 사용된다.
  get firstElem() {
    // getter는 반드시 무언가를 반환해야 한다.
    return this._arr.length ? this._arr[0] : null;
  }
}

const foo = new Foo([1, 2]);
```

### setter

```javascript
class Foo {
  constructor(arr = []) {
    this._arr = arr;
  }

  // getter: get 키워드 뒤에 오는 메소드 이름 firstElem은 클래스 필드 이름처럼 사용된다.
  get firstElem() {
    // getter는 반드시 무언가를 반환해야 한다.
    return this._arr.length ? this._arr[0] : null;
  }

  set firstElem(elem) {
    this._arr = [elem, ...this._arr];
  }
}

const foo = new Foo([1, 2]);

foo.firstElem = 100;

console.log(foo.firstElem);
```

### static

- static 메소드는 인스턴스를 생성하지 않아도 되고, this를 사용할 수 없다.
- static 코드를 ES5 문법으로 표현해서 보면 쉽게 이해할 수 있다.

```javascript
var Foo = (function () {
  // 생성자 함수
  function Foo(prop) {
    this.prop = prop;
  }

  Foo.staticMethod = function () {
    return "staticMethod";
  };

  Foo.prototype.prototypeMethod = function () {
    return this.prop;
  };

  return Foo;
})();

var foo = new Foo(123);
console.log(foo.prototypeMethod()); // 123
console.log(Foo.staticMethod()); // staticMethod
console.log(foo.staticMethod()); // Uncaught TypeError: foo.staticMethod is not a function
```

### extends 키워드

- extends 키워드는 부모 클래스를 상속받는 자식 클래스를 정의할 때 사용한다.

```javascript
class Parent {}

class Child extends Parent {}

console.log(Child.__proto__ === Parent); // true
console.log(Child.prototype.__proto__ === Parent.prototype); // true
```

## Array

- 자바스크립트의 배열은 객체이다.
- 배열의 길이는 마지막 인덱스를 기준으로 산정된다.
- 값이 할당되지 않은 인덱스 위치의 요소는 생성되지 않는다는 것에 주의한다.
  (단, 존재하지 않는 요소를 참조하면 undefined가 반환된다.)
- 배열 요소를 삭제하기 위해서는 Array.prototype.splice 메소드를 사용한다.
- 배열의 순회에는 forEach 메소드, for문, for...of 문을 사용한다.

### 고차 함수

- **함수를 인자**로 전달받거나 **함수를 결과**로 반환하는 함수를 말한다.
- 자바스크립트는 고차 함수를 다수 지원하고 있다.
- 특히, Array 객체는 매우 유용한 고차 함수를 제공한다.

#### 정렬

- 기본 정렬 순서는 문자열 Unicode 코드 포인트 순서에 따른다.
- 배열의 요소가 숫자 타입이라 할지라도 배열의 요소를 일시적으로 문자열로 변환한 후, 정렬한다.

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

points.sort();
console.log(points); // [ 1, 10, 100, 2, 25, 40, 5 ]
```

- 이러한 경우, sort 메소드의 인자로 정렬 순서를 정의하는 비교 함수를 인수로 전달한다.

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

points.sort(function (a, b) {
  // 0보다 작은 경우 a우선
  return a - b;
});

console.log(points);

points.sort(function (a, b) {
  // 0보다 큰 경우 b우선
  return b - a;
});

console.log(points);
```

- 객체를 요소를 갖는 배열을 정렬하는 예제

```javascript
const todos = [
  { id: 4, content: "JavaScript" },
  { id: 1, content: "HTML" },
  { id: 2, content: "CSS" },
];

function compare(key) {
  return function (a, b) {
    return a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0;
  };
}

todos.sort(compare("content"));

console.log(todos);
```

#### forEach

```javascript
const numbers = [1, 2, 3, 4];

// forEach 메소드는 원본 배열(this)을 변경하지 않는다. 하지만 콜백 함수는 원본 배열(this)을 변경할 수는 있다.
// 원본 배열을 직접 변경하려면 콜백 함수의 3번째 인자(this)를 사용한다.
numbers.forEach(function (item, index, self) {
  self[index] = Math.pow(item, 2);
});

console.log(numbers); // [ 1, 4, 9, 16 ]
```

- forEach 두번째 인자로 this를 전달할 수 있다.

```javascript
function Square() {
  this.array = [];
}

Square.prototype.multiply = function (arr) {
  arr.forEach(function (item) {
    this.array.push(item * item);
  }, this); // this를 인수로 전달하지 않으면 this는 window 이다.
};

const square = new Square();
square.multiply([1, 2, 3]);
console.log(square.array);
```

- myForEach 구현

```javascript
Array.prototype.myForEach = function (f) {
  for (let i = 0; i < this.length; i++) {
    // 배열 요소의 값, 요소 인덱스, forEach 메소드를 호출한 배열, 즉 this를 매개변수에 전달하고 콜백 함수 호출
    f(this[i], i, this);
  }
};

[0, 1, 2, 3].myForEach(function (item, index, array) {
  console.log(`[${index}]: ${item} of [${array}]`);
});
```

#### reduce

![image](https://user-images.githubusercontent.com/68647194/104836393-12c20580-58f1-11eb-9e69-f9bfc6af7bff.png)

```javascript
const arr = [1, 2, 3, 4, 5];

const sum = arr.reduce(function (pre, cur) {
  return pre + cur;
}, 5);

console.log(sum);
```

- 객체의 프로퍼티 값을 합산하는 경우

```javascript
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 },
];

const sum = products.reduce(function (pre, cur) {
  return pre + cur.price;
}, 0);

console.log(sum);
```

💡 참고 자료

- 부스트코스 웹 프로그래밍(풀스택) 강의<br>
- https://webclub.tistory.com/1
- https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67
- https://www.zerocho.com/category/JavaScript
- https://poiemaweb.com/es6-block-scope
