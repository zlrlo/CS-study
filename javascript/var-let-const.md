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
  - 초기화 단계 : **메모리에 공간을 확보하는 단계** (undefined 할당)
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

- for 루프의 let i는 for loop에서만 유효한 지역 변수이다. 또한 i는 자유 변수로서 for 루프의 생명주기가 종료되어도 변수 i를 참조하는 함수가 존재하는 한 계속 유지된다. 

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