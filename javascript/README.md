# Javascript

## 자바스크립트 버전

- 자바스크립트 버전은 ECMAScript(ES)의 버전에 따라서 결정된다.
- 이를 자바스크립트 실행 엔진이 반영한다. (브라우저마다 자바스크립트 실행 엔진이 있다)

## 연산자 - 비교 연산자

- 비교는 `==` 보다는 `===`를 사용한다.
- == 로 인한 다양한 오류 상황이 있다.
- 암묵적으로 타입을 바꿔서 비교한다.

```javascript
0 == false; // true
"" == false; // true
null == false; // false
0 == "0"; // true
null == undefined; // true
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
  - 정의되지 않은 것, 초기화 되어 있지 않거나 존재하지 않는
  - 데이터 타입이자, 값
- `null`

  - 아무 값도 갖지않음을 나타낸다.
  - 주로 객체를 담을 변수를 초기화할 때 많이 사용한다.
  - typeof null은 object

  ```javascript
  var data1 = 0; //Number 변수 초기화
  var data2 = ""; // String 변수 초기화
  var data3 = false; // Boolean 변수 초기화
  var data4 = null; // Object 변수 초기화
  ```

- undefined는 변수를 선언만 하더라도 할당되지만 null은 변수를 선언한 후에 null로 값을 바꾼다.
- undefined는 미리 선언된 전역변수(전역 객체의 프로퍼티)이며, null은 선언, 등록을 하는 키워드

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
- 객체는 언제나 함수(Function)으로 생성된다.<br> ex) var personObject = new Person();
- Object()는 자바스크립트에서 기본적으로 제공하는 함수이다.<br> ex) var obj = new Object();
- Constructor 자격이 부여되면 `new` 키워드 사용가능 (함수만 new 키워드 사용 가능)
  - Constructor : function A() -> function A()를 자신의 프로토타입으로 사용하여 만들어졌다.
- 생성된 함수는 prototype이라는 속성을 통해 Prototype Object에 접근 할 수 있다.<br>
  - prototype -> Object 객체
- Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 `constructor`와 `__proto__`를 가지고 있다.
- 모든 객체는 `__proto__` 속성을 가지고 있다.
- `__proto__`는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킨다.
- 최상위인 Object의 Prototype Object까지 도달했는데도 못찾았을 경우 `undefined` 리턴

## 클로저

- 클로저 안에 정의된 함수는 만들어진 환경을 기억한다.
- (환경을 만들어주고 함수를 반환한다.)

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

## this

- this는 기본적으로 window 이다.
- 웹 브라우저에서는 window 객체가 전역 객체이다.
- 객체 메서드 안에 this는 객체를 가리킨다.
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

## computed property name

💡 참고 자료

- 부스트코스 웹 프로그래밍(풀스택) 강의<br>
- https://webclub.tistory.com/1
- https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67
- https://hyunseob.github.io/2016/08/30/javascript-closure/
- https://www.zerocho.com/category/JavaScript/post/5b0645cc7e3e36001bf676eb
