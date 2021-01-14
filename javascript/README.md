# JavaScript

## 자바스크립트 버전

- 자바스크립트 버전은 ECMAScript(ES)의 버전에 따라서 결정된다.
- 이를 자바스크립트 실행 엔진이 반영한다. (브라우저마다 자바스크립트 실행 엔진이 있다)

## 연산자 - 비교 연산자

- 비교는 `==` 보다는 `===`를 사용한다.
- == 으로 인한 다양한 오류 상황이 있다.
- 암묵적으로 타입을 바꿔서 비교한다.

```javascript
0 == false; // true
"" == false; // true
0 == "0"; // true
null == undefined; // true
null == false; // false
```

## 타입

- **타입은 선언할때가 아니고, 실행타임에 결정된다.**

```javascript
function run(a) {
  console.log(typeof a);
}

run("hello"); // 실행타임에 a 타입 결정
```

### `undefined` 와 `null` 의 차이점

- `undefined`

  - 정의되지 않은 것, **초기화** 되어 있지 않거나 존재하지 않는
  - 데이터 **타입이자, 값**

  ```javascript
  var hello;
  console.log(typeof hello); // undefined
  ```

- `null`

  - 아무 값도 갖지않음을 나타낸다.
  - 주로 **객체**를 담을 변수를 초기화할 때 많이 사용한다.
  - **typeof null은 object**

  ```javascript
  var data1 = 0; //Number 변수 초기화
  var data2 = ""; // String 변수 초기화
  var data3 = false; // Boolean 변수 초기화
  var data4 = null; // Object 변수 초기화
  ```

- undefined는 변수를 선언만 하더라도 할당되지만 null은 변수를 선언한 후에 null로 값을 바꾼다.

## 프로토타입

- 자바스크립트는 프로토타입 기반 언어
- **함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성된다.**
- **어떠한 객체가 만들어지기 위해 그 객체의 모태가 되는 녀석을 프로토타입이라고 한다.**

```javascript
    function Person() {}

    Person.prototype.eyes = 2;
    Person.prototype.nose = 1;

    var kim  = new Person();
    var park = new Person():

    console.log(kim.eyes); // => 2
    ...
```

- Person.prototype이라는 빈 Object가 어딘가에 존재하고, Person함수로부터 생성된 객체(kim, park) 들은 어딘가에 존재하는 Object에 들어있는 값을 모두 갖다쓸 수 있다.
- 객체는 언제나 함수(Function)으로 부터 생성된다.<br> ex) `var personObject = new Person();`
- Object()는 자바스크립트에서 기본적으로 제공하는 함수이다.<br> ex) `var obj = new Object();`
- 생성된 함수는 prototype이라는 속성을 통해 Prototype Object에 접근 할 수 있다.
- Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 `constructor`와 `__proto__`를 가지고 있다.
- 모든 객체는 `__proto__` 속성을 가지고 있다.
- `__proto__`는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킨다.
- 최상위인 Object의 Prototype Object까지 도달했는데도 못찾았을 경우 `undefined` 리턴

## Object

- 모든 객체의 최종 prototype
- Object의 `__proto__` 는 null (최종이기 때문)
- Object 객체의 생성자는 window 객체에 저장되어 있다.
- 모든 객체가 Object 객체로부터 상속받기 때문에 모든 객체는 Object 객체의 메소드를 사용할 수 있다.

### Object.create(prototype, 속성)

- 객체를 생성하는 방법 중 하나
- prototype을 설정할 수 있다.

```javascript
var obj = {}; // Object.create(Object.prototype); 과 같음
```

## this

- this는 기본적으로 window 이다.
- 웹 브라우저에서는 window 객체가 전역 객체이다.
- 객체 메서드 안에 this는 객체를 가리킨다.<br>
  (메서드를 호출할 때, this를 내부적으로 바꿔주기 때문)
- **호출할 때, 호출하는 함수가 객체의 메서드인지 그냥 함수인지가 중요하다.**

```javascript
// 주의
var a2 = obj.a;
a2(); // window
```

- 명시적으로 this를 바꿀 수 있다. (bind, call, apply)
- 생성자 함수에 new를 붙이면 this가 생성자를 통해 생성된 인스턴스가 된다.

```javascript
var hero = new Person("Hero", 33); // Person {name: "Hero", age: 33}
hero.sayHi(); // Hero 33
```

- 이벤트리스너에서 this
  - 이벤트가 발생할 때, 내부적으로 this가 바뀐다.

```javascript
document.body.onclick = function () {
  console.log(this); // <body>
};
```

- 아래와 같은 문제 발생 가능
  (inner함수 안에 있는 this와 밖에 있는 this가 다름)

```javascript
$("div").on("click", function () {
  console.log(this); // <div>
  function inner() {
    // 주의 : this는 기본적으로 window
    console.log("inner", this); // inner Window
  }
  inner();
});
```

- 해결 방법
  - that이라는 변수를 만들어서 this를 저장한다.
  - 화살표 함수를 쓴다. (화살표 함수는 상위 함수의 this를 가져온다.)

```javascript
$("div").on("click", function () {
  console.log(this); // <div>
  var that = this;
  function inner() {
    console.log("inner", that); // inner <div>
  }
  inner();
});
```

```javascript
$("div").on("click", function () {
  console.log(this); // <div>
  const inner = () => {
    console.log("inner", this); // inner <div>
  };
  inner();
});
```

## Window 객체

- 윈도우 전체를 담당하는게 Window 객체이고, 웹사이트만 담당하는게 Document 객체라고 이해하면 된다.
- Documnet도 Window 객체 안에 있다.
- window는 생략 가능 ex) `window.parseInt() -> parseInt()`

## Scope

- 자바스크립트는 변수의 범위를 호출한 함수의 지역 스코프부터 전역 변수들이 있는 전역 스코프까지 점차 **넓혀가며 찾는다.**

```javascript
var x = "global";
function ex() {
  var x = "local";
  x = "change";
}
ex(); // x를 바꿔본다.
alert(x); // 여전히 'global'
```

```javascript
var x = "global";
function ex() {
  x = "change";
}
ex();
alert(x); // 'change'
```

### Scope chain

- **내부 함수에서는 외부 함수의 변수에 접근 가능**하지만 외부 함수에서는 내부 함수의 변수에 접근할 수 없다.
- **모든 함수들은 전역 객체에 접근할 수 있다.**
- 꼬리를 물고 계속 범위를 넓히면서 찾는 관계를 **스코프 체인**이라고 부른다.

```javascript
var name = "zero";
function outer() {
  console.log("외부", name);
  function inner() {
    var enemy = "nero";
    console.log("내부", name);
  }
  inner();
}
outer();
console.log(enemy); // undefined
```

### 전역변수를 지양해야 하는 이유

- 변수가 섞일 수 있다.
- 여러 명과 협동하거나, 다른 사람의 라이브러리를 사용하다보면 우연의 일치로 인해 같은 변수 이름을 사용해서 이전에 있던 변수를 덮어쓰는 불상사가 발생할 수 있다. (특히 $나 \_같은 유니크한 변수들은 경쟁이 치열하다)

#### 해결 방법

- 전역 변수 대신 객체 안의 속성으로 만든다.
- 이런 방법을 네임스페이스를 만든다고 표현한다.
- 네이버는 jindo, facebook은 FB, jquery는 jQuery(또는 $)

```javascript
var obj = {
  x: "local",
  y: function () {
    alert(this.x);
  },
};
```

- 하지만 obj.x = 'hacked'; 라고 한 줄 추가만 하면 obj.y()의 결과가 달라진다.

```javascript
var another = function () {
  var x = "local";
  function y() {
    alert(x);
  }
  return { y: y };
};
var newScope = another();
```

- y는 공개 변수이고 x는 비공개 변수가 된다.

```javascript
var newScope = (function () {
  var x = "local";
  return {
    y: function () {
      alert(x);
    },
  };
})(); // 즉시 실행 함수
```

## Lexical scoping (렉시컬 스코핑)

- 스코프는 함수를 호출할 때가 아니라 **선언**할 때 생긴다.

## 실행 컨텍스트

```javascript
var name = 'zero'; // (1)변수 선언 (6)변수 대입
function wow(word) { // (2)변수 선언 (3)변수 대입
  console.log(word + ' ' + name); // (11)
}
function say () { // (4)변수 선언 (5)변수 대입
  var name = 'nero'; // (8)
  console.log(name); // (9)
  wow('hello'); // (10)
}
say(); // (7)this: window,
}
```

### 전역 컨텍스트

- 브라우저가 스크립트를 로딩해서 실행하는 순간 모든 것을 포함하는 전역 컨텍스트가 생긴다.

```javascript
'전역 컨텍스트': {
  변수객체: {
    arguments: null,
    variable: ['name', 'wow', 'say'],
  },
  scopeChain: ['전역 변수객체'],
  this: window,
}
```

### 함수 컨텍스트

- `say();` 를 하는 순간 새로운 컨텍스트인 say함수 컨텍스트가 생긴다.

```javascript
'say 컨텍스트': {
  변수객체: {
    arguments: null,
    variable: ['name'], // 초기화 후 [{ name: 'nero' }]가 됨
  },
  scopeChain: ['say 변수객체', '전역 변수객체'],
  this: window,
}
```

- `wow();` 를 하는 순간 새로운 컨텍스트인 wow함수 컨텍스트가 생긴다.
- wow 함수의 스코프 체인은 선언 시에 이미 정해져있다.
- **따라서 say 스코프는 wow 컨텍스트의 scope chain이 아니다.**

```javascript
'wow 컨텍스트': {
  변수객체: {
    arguments: [{ word : 'hello' }],
    variable: null,
  },
  scopeChain: ['wow 변수객체', '전역 변수객체'],
  this: window,
}
```

- **컨텍스트가 생긴 후 함수가 실행된다.**
- **함수 종료 후 컨텍스트가 사라진다.**

## 호이스팅

- 변수를 선언하고 초기화했을 때 **선언 부분이 최상단**으로 끌어올려지는 현상을 의미한다. (초기화 또는 대입 부분은 그대로 남아있다.)
- 함수 선언식일 경우 컨텍스트 생성 후 바로 대입된다.

```javascript
// 문제가 발생한는 경우
sayWow(); // (3)
sayYeah(); // (5) 여기서 대입되기 전에 호출해서 에러
var sayYeah = function () {
  // (1) 선언 (6) 대입
  console.log("yeah");
};
function sayWow() {
  // (2) 선언과 동시에 초기화(호이스팅)
  console.log("wow"); // (4)
}
```

## 클로저

- return안에 들어 있는 함수
- 비공개 변수를 만들 수 있다.
- 프로그램 사용자는 공개한 메소드만 사용한다.
- 사용자를 통제하기 위한 기본적인 방법이 바로 클로저이다.

```javascript
"closure 컨텍스트":  {
  변수객체: {
    arguments: null,
    variable: null,
  scopeChain: ['closure 변수객체', 'makeClosure 변수객체', '전역 변수객체'],
  this: window,
}
```

```javascript
function getClosure() {
  var text = "variable 1";
  return function () {
    return text;
  };
}

var closure = getClosure();
console.log(closure());
```

- 클로저를 통한 은닉화

```javascript
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function () {
  console.log("Hello, " + this._name);
};

var hello1 = new Hello("지은");

hello1.say();
// _name 변수에 외부에서도 쉽게 접근 가능
hello1._name = "anonymouse";
hello1.say();
```

```javascript
// _name 변수에 접근할 방법이 전혀 없다.
function hello(name) {
  var _name = name;
  return function () {
    console.log("Hello, " + _name);
  };
}

var hello1 = hello("지은");

hello1();
```

- 반복문 클로저

```javascript
var i;

for (i = 0; i < 10; i++) {
  (function (j) {
    setTimeout(function () {
      console.log(j);
    }, 100);
  })(i);
}
```

## var, let, const 차이점

### var

- 함수 레벨 스코프
  - 함수의 코드 블록만을 스코프로 인정한다.
  - 전역 함수 외부에서 생성한 변수는 모두 전역 변수이다.
  - for문의 변수 선언문에서 선언한 변수를 for문의 코드 블록 외부에서 참조할 수 있다.
- var 키워드 생략 허용
  - 암묵적 전역 변수를 양산할 가능성이 크다.
- 변수 중복 선언 허용
  - 의도하지 않은 변수값의 변경이 일어날 가능성이 크다.
- 변수 호이스팅
  - 변수를 선언하기 전에 참조할 수 있다.

> 전역 변수는 scope가 넓어서 어디에서 어떻게 사용될 것인지 파악하기 힘들며, 의도하지 않게 변경될 수도 있어서 복잡성을 증가시키는 원인이 된다. 따라서 변수의 스코프는 좁을수록 좋다.

### let

- 블록 레벨 스코프
- 변수 중복 선언 금지
- 호이스팅 한다. 하지만

  - 선언문 이전에 참조하면 참조 에러가 발생한다.
  - 이는 let 키워드로 선언된 변수는 스코프 시작에서 변수의 선언까지 **일시적 사각지대(TDZ)**에 빠지기 때문이다.
  - var 키워드로 선언된 변수는 선언 단계와 초기화 단계가 한번에 이루어진다.
  - let 키워드로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.
  - 초기화 단계 : 메모리에 공간을 확보하는 단계 (undefined 할당)
  - 결국 ES6에서는 호이스팅이 발생하지 않는 것과 차이가 없어 보인다. 하지만 그렇지 않다.

  ```javascript
  let foo = 1; // 전역 변수

  {
    console.log(foo); // ReferenceError: foo is not defined
    let foo = 2; // 지역 변수
  }
  ```

  ![image](https://user-images.githubusercontent.com/68647194/104606033-99dd6680-56c2-11eb-8c20-2a783fdcb13b.png)

#### var, let 호이스팅 비교 예제

```javascript
// 스코프의 선두에서 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.

console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

```javascript
// 스코프의 선두에서 선언 단계가 실행된다.
// 아직 변수가 초기화(메모리 공간 확보와 undefined로 초기화)되지 않았다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 없다.

console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

- 햇갈릴 수 있는 예제

```javascript
var funcs = [];

// 함수의 배열을 생성하는 for 루프의 i는 전역 변수다.
for (var i = 0; i < 3; i++) {
  funcs.push(function () {
    console.log(i); // 전역 변수 i 대입!
  });
}

// 배열에서 함수를 꺼내어 호출한다.
for (var j = 0; j < 3; j++) {
  funcs[j](); // 3이 3번 호출된다.
}
```

- 전역 객체와 let
  - 전역 객체는 모든 객체의 유일한 최상위 객체를 의미하며 일반적으로 Browser-side 에서는 window 객체, Server-side(Node.js)에서는 global객체를 의미한다.
  - let 키워드로 선언된 변수를 전역 변수로 사용하는 경우, let 전역 변수는 전역 객체의 프로퍼티가 아니다.
  - 즉, window.foo와 같이 접근할 수 없다.
  - let 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다.

```javascript
var foo = 123; // 전역변수
console.log(window.foo); // 123
```

```javascript
let foo = 123; // 전역변수
console.log(window.foo); // undefined
```

### const와 객체

- const는 재할당이 금지된다.
- const 변수의 타입이 객체인 경우, 객체에 대한 참조를 변경하지 못한다는 것을 의미한다.
- 하지만 객체의 프로퍼티는 보호되지 않는다.
- 즉, 재할당은 불가능하지만 할당된 객체의 내용은 변경할 수 있다.
- 객체의 내용이 변경되더라도 객체 타입 변수에 할당된 주소값은 변경되지 않는다.
- 따라서 객체 타입 변수 선언에는 const를 사용하는 것이 좋다.
- 만약에 명시적으로 객체 타입 변수의 주소값을 변경(재할당)하여야 한다면 let을 사용한다.

```javascript
const user = { name: "Lee" };

// const 변수는 재할당이 금지된다.
// user = {}; // TypeError: Assignment to constant variable.

// 객체의 내용은 변경할 수 있다.
user.name = "Kim";

console.log(user); // { name: 'Kim' }
```

## computed property name

💡 참고 자료

- 부스트코스 웹 프로그래밍(풀스택) 강의<br>
- https://webclub.tistory.com/1
- https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67
- https://www.zerocho.com/category/JavaScript
- https://poiemaweb.com/es6-block-scope
