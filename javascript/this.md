## this

- 함수와 메서드의 구분이 느슨한 자바스크립트에서 this는 실질적으로 이 둘을 구분하는 거의 유일한 기능이다. 
- 자바스크립트에서 this는 기본적으로 실행 컨텍스트가 생성될 때 함께 결정된다. 
- 실행 컨텍스트는 함수를 호출할 때 생성되므로, 바꿔 말하면 **this는 함수를 호출할 때 결정된다고 할 수 있다.**
- this는 기본적으로 window 이다.


### Window 객체

- 윈도우 전체를 담당하는게 Window 객체이고, 웹사이트만 담당하는게 Document 객체라고 이해하면 된다.
- Documnet도 Window 객체 안에 있다.
- window는 생략 가능 ex) `window.parseInt() -> parseInt()`

### 전역 공간에서의 this

- 전역 공간에서 this는 전역 객체를 가리킨다. 
- 자바스크립트의 모든 변수는 실은 **특정 객체의 프로퍼티**로서 동작한다.
- 특정 객체란 바로 실행 컨텍스트의 LexicalEnvironment(L.E)이다. 
- 전역 컨텍스트의 경우 L.E는 전역객체를 그대로 참조한다. 

### 메서드로서 호출할 때 그 메서드 내부에서 this

- 함수와 메서드를 구분하는 유일한 차이는 독립성에 있다. 
- 함수는 그 자체로 독립적인 기능을 수행하는 반면, 메서드는 자신을 호출한 대상 객체에 관한 동작을 수행한다. 
- 어떤 함수를 호출할 때 그 함수 이름 앞에 객체가 명시돼 있는 경우에는 메서드로 호출한 것이고, 그렇지 않은 모든 경우에는 함수로 호출한 것이다. 
- 어떤 함수를 메서드로서 호출하는 경우 호출 주체는 바로 함수명 앞의 객체이다. 
- **점 표기법의 경우 마지막 점 앞에 명시된 객체가 곧 this가 되는 것이다.** 

### 함수로서 호출할 때 그 함수 내부에서의 this

- this 바인딩에 관해서는 함수를 실행하는 당시의 주변 환경(메서드 내부인지, 함수 내부인지 등)은 중요하지 않고, 오직 해당 함수를 호출하는 구문 앞에 점 또는 대괄호 표기가 있는지 없는지가 관건이다. 

```javascript
var obj1 = {
    outer: function() {
        console.log(this); // obj1

        var innerFuc = function() {
            console.log(this); // this, obj2
        }

        innerFuc(); 

        var obj2 = {
            innerMathod: innerFuc
        };

        obj2.innerMathod();
    }
}

obj1.outer();
```

### 메서드 내부 함수에서 this 우회하는 방법

- `self`가 가장 많이 쓰인다. 

```javascript
var self = this; 

var innerFunc2 = function() {
    console.log(self);
}

innserFunc2(); 
```

### 화살표 함수

- 화살표 함수는 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지게 되었다. 
- 즉 이 함수 내부에는 this가 아예 없으며, 접근하고자 한다면 **스코프체인상 가장 가까운 this에 접근하게 된다.**

### 콜백함수

- 함수 A의 제어권을 다른 함수 B에게 넘겨주는 경우, 함수 A를 콜백 함수라고 한다. 
- 이때, 함수 A는 함수 B의 내부 로직에 따라 실행된다.
- 콜백 함수의 제어권을 가지는 함수가 콜백 함수에서의 this를 무엇으로 할지를 결정하며, 특별히 정의하지 않은 경우에는 기본적으로 함수와 마찬가지로 전역객체를 바라본다. 

#### addEventListener

- addEventListener 메서드는 콜백 함수를 호출할 때 자신의 this를 상속하도록 정의돼 있다. 
- 메서드의 점(.) 앞부분이 곧 this가 된다. 

```javascript
document.body.onclick = function () {
  console.log(this); // <body>
};
```

### 생성자 함수 내부에서의 this

- 생성자 함수는 어떤 공통된 성질을 지니는 객체들을 생성하는데 사용하는 함수이다. 
- 객체지향 언어에서는 생성자를 클래스, 클래스를 통해 만든 객체를 인스턴스라고 한다. 
- 자바스크립트는 함수에 생성자로서의 역할을 함께 부여했다. new 명령어와 함께 함수를 호출하면 해당 함수가 생성자로서 동작한다. 
- 어떤 함수가 생성자 함수로서 호출된 경우 내부에서의 this는 곧 새로 만들 구체적인 인스턴스 자신이 된다. 
- 생성자 함수를 new 명령어와 함께 함수를 호출하면 우선 생성자의 prototype 프로퍼티를 참조하는 __proto__라는 프로퍼티가 있는 인스턴스를 만들고, 미리 준비된 공통 속성 및 개성을 this에 부여한다. 

> 객체 리터럴 방식의 경우, 생성된 객체의 프로토타입 객체는 **Object.prototype**이고, 생성자 함수 방식의 경우, 생성된 객체의 프로토타입 객체는 **Person.prototype**이다.

### 명시적으로 this를 바인딩하는 방법

- 규칙을 깨고 this에 별도의 대상을 바인딩하는 방법도 있다. 

#### call 메서드

- call 메서드는 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령이다. 
- 이때 call 메서드의 첫 번째 인자를 this로 바인딩하고, 이후의 인자들은 호출할 함수의 매개변수로 한다. 

```javascript
var func = function(a, b, c) {
    console.log(this, a, b, c);
}

func(1, 2, 3);

func.call({ x: 1 }, 4, 5, 6);
```

#### apply 메서드

- call 메서드와 기능적으로 완전히 동일하다.
- apply 메서드는 두 번째 인자를 **배열**로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다. 

#### 유사배열객체에 배열 메서드를 적용

- 객체에는 배열 메서드를 직접 적용할 수 없다. 
- 그러나 키가 0 또는 양의 정수인 프로퍼티가 존재하고 length 프로퍼티의 값이 0 또는 양의 정수인 객체, 즉 배열의 구조와 유사한 객체의 경우 call 또는 apply 메서드를 이요해 배열 메서드를 차용할 수 있다. 

```javascript
var obj = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3
};

Array.prototype.push.call(obj, 'd');

console.log(obj);
// { 0: 'a', 1: 'b', 2: 'c', 3: 'd', length: 4 }

var arr = Array.prototype.slice.call(obj);

console.log(arr);
// ['a', 'b', 'c', 'd']
```

- arguments, NodeList에 배열 메서드를 적용할 때도 가능!

#### Array.from

- ES6에서는 유사배열객체 또는 순회 가능한 모든 종류의 데이터 타입을 배열로 전환하는 Array.from 메서드를 새로 도입했다.

#### bind

- call과 비슷하지만 즉시 호출하지는 않고 넘겨 받은 this 및 인수들을 바탕으로 **새로운 함수를 반환**하기만 하는 메서드이다.
- bind 메서드는 함수에 **this를 미리 적용하는 것**과 **부분 적용 함수**를 구현하는 두 가지 목적을 가진다. 
- bind 메서드를 적용하면 name 프로퍼티에 동사 bind의 수동태인 bound라는 접두어가 붙는다.  

```javascript
var func = function(a, b, c, d) {
    console.log(this, a, b, c, d);
}

func(1, 2, 3, 4); // window 1 2 3 4

var bindFunc1 = func.bind({x:1});
bindFunc1(5, 6, 7, 8); // { x: 1 } 5 6 7 8

console.log(bindFunc1); // bound func
```

#### 별도의 인자로 this를 받는 경우

- 여러 내부 요소에 대해 간단한 동작을 반복 수행해야 하는 배열 메서드에 많이 포진돼 있다. 
- `forEach`, `map`, `filter`, `some`, `every`, `find`, `findIndex`, `flatMap`, `from`...

```javascript
Array.prototype.forEach(callback[, thisArg])
```

💡 참고 자료

- [도서] 코어 자바스크립트 
- https://poiemaweb.com/




