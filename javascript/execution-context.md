## 실행 컨텍스트

- 실행할 코드에 제공할 환경 정보들을 모아놓은 **객체**
- 자동으로 생성되는 전역공간과 eval을 제외하면 우리가 흔히 실행 컨텍스트를 구성하는 방법은 **함수를 실행**하는 것뿐이다.
- 콜 스택에 쌓아올린다. 
- **컨텍스트가 생긴 후 함수가 실행된다.**
- **함수 종료 후 컨텍스트가 사라진다.**

### 활성화된 실행 컨텍스트의 수집 정보

- VariableEnvironment
- LexicalEnvironment
- ThisBinding

### VariableEnvironment

```
environmentRecord (snapshot)
outerEnvironmentReference (snapshot)
```

### LexicalEnvironment ⭐️

```
environmentRecord // 식별자 정보들이 저장된다. 
outerEnvironmentReference // 스코프, 스코프 체인
```

- 사전적 환경
- 컨텍스트를 구성하는 환경 정보들을 사전에서 접하는 느낌으로 모아놓은 것

#### environmentRecord와 호이스팅

- 현재 컨텍스트와 관련된 코드의 **식별자 정보**들이 저장된다.
- 컨텍스트 내부 전체를 처음부터 끝까지 훑어나가며 순서대로 수집한다. 
- 따라서 '자바스크립트 엔진은 식별자들을 최상단으로 끌어올려놓은 다음 실제 코드를 실행한다.' 라고 생각하더라도 코드를 해석하는 데는 문제될 것이 전혀 없다.
- 자바스크립트 엔진이 실제로 끌어올리지는 않지만 편의상 끌어올린 것으로 표현한다. 

##### 실행 결과 예상해보기

```javascript
// 원본 코드
function a(x) {
    console.log(x); // 1

    var x; 

    console.log(x);  // 1

    var x = 2; 

    console.log(x); // 2
}

a(1);
```

```javascript
// 호이스팅을 마친 상태
function a() {
    var x;
    var x; 
    var x;
    
    x = 1; 

    console.log(x); // 1 
    console.log(x);  // 1

    x = 2; 

    console.log(x); // 2
}

a(1);
```

```javascript
// 원본 코드
function a() {
    console.log(b); // b()
    var b = 'bbb';
    console.log(b); // 'bbb'
    function b() {}
    console.log(b); // 'bbb'
}

a(); 
```

```javascript
// 호이스팅을 마친 상태
function a() {
    var b; // 변수는 선언부만 끌어올린다. 
    var b = function b() {} // 함수 선언은 전체를 끌어올린다. 

    console.log(b); // b()
    b = 'bbb';
    console.log(b); // 'bbb'
    console.log(b); // 'bbb'
}

a(); 
```

> 함수 선언문은 큰 혼란을 일으키는 원인이 되기도 한다.

##### 함수 선언문의 위험성

```javascript
console.log(sum(3, 4)); // 3 + 4 = 7

function sum(x, y) {
    return x + y; 
}

var a = sum(1, 2);

function sum(x, y) {
    return `${x} + ${y} = ${x + y}`;
}

var c = sum(1, 2); // 1 + 2 = 3

console.log(c);
```

- 동일한 변수명에 서로 다른 값을 할당할 경우 나중에 할당한 값이 먼저 할당한 값을 덮어씌운다. (override)

##### 상대적으로 함수 표현식이 안전한다. 

```javascript
console.log(sum(3, 4)); // sum is not a function

var sum = function(x, y) {
    return x + y; 
}

var a = sum(1, 2);

var sum = function(x, y) {
    return `${x} + ${y} = ${x + y}`;
}

var c = sum(1, 2);

console.log(c);
```

- 더욱 빠른 타이밍에 손쉽게 디버깅할 수 있다. 

### outerEnvironmentReference

- 스코프란 식별자에 대한 유효범위이다.
- A의 외부에서 선언한 변수는 A의 외부뿐 아니라 A의 내부에서도 접근이 가능한다. 
- A의 내부에서 선언한 변수는 오직 A의 내부에서만 접근할 수 있다. 
- ES5까지의 자바스크립트는 전역공간을 제외하면 오직 함수에 의해서만 스코프가 생성된다. 
- **식별자의 유효범위를 안에서부터 바깥으로 차례로 검색해나가는 것을 스코프 체인이라고 한다.**

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

#### 스코프 체인

- outerEnvironmentReference는 현재 호출된 함수가 **선언될 당시**의 LexicalEnvironment를 참조한다. 
- 예를 들어, A 함수 내부에 B 함수를 선언하고 다시 B 함수 내부에 C 함수를 선언한 경우, 함수 C의 outerEnvironment는 함수 B의 LexicalEnvironment를 참조한다.
- 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능하게 된다. 
- **모든 함수들은 전역 객체에 접근할 수 있다.**
- 꼬리를 물고 계속 범위를 넓히면서 찾는 관계를 **스코프 체인**이라고 부른다.

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

- `wow();` 를 하는 순간 새로운 컨텍스트인 wow함수 컨텍스트가 생긴다.
- **say 스코프는 wow 컨텍스트의 scope chain이 아니다.**

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

#### 전역변수와 지역변수

- 전역 공간에서 선안한 변수는 전역변수이고, 함수 내부에서 선언한 변수는 무조건 지역변수이다. 
- 코드의 안전성을 위해 가급적 전역변수 사용을 최소화하고자 노력해야한다. 
- ex) A가 sum 함수를 지역변수로 선언하기 위해 외부에 X 라는 함수를 하나 더 만드는 순간, sum 함수를 호출할 수 있는 영역은 오직 X 내부로 국한된다. 
- ex) 여러 명과 협동하거나, 다른 사람의 라이브러리를 사용하다보면 우연의 일치로 인해 같은 변수 이름을 사용해서 이전에 있던 변수를 덮어쓰는 불상사가 발생할 수 있다. (특히 $나 \_같은 유니크한 변수들은 경쟁이 치열하다)

#### 전역 변수 문제 해결 방법

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

💡 참고 자료

- [도서] 코어 자바스크립트 
- https://poiemaweb.com/






















