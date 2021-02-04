## 실행 컨텍스트

- 실행할 코드에 제공할 환경 정보들을 모아놓은 **객체**
- 자동으로 생성되는 전역공간과 eval을 제외하면 우리가 흔히 실행 컨텍스트를 구성하는 방법은 **함수를 실행**하는 것뿐이다.
- 콜 스택에 쌓아올린다. 

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












💡 참고 자료

- [도서] 코어 자바스크립트 




