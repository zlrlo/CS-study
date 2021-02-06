## 콜백 함수

- 콜백 함수는 다른 코드의 인자로 넘겨주는 함수이다. 
- 콜백 함수를 넘겨받은 코드는 이 콜백 함수를 필요에 따라 적절한 시점에 실행한다. 
- 콜백 함수는 다른 함수에게 인자로 넘겨줌으로써 그 제어권도 함께 위임한 함수이다. 
- 콜백 함수를 위임받은 코드는 자체적인 내부 로직에 의해 이 콜백 함수를 적절한 시점에 실행할 것이다.

### 콜백 함수 예제

- Array.prototype.map - 구현하기
```javascript
Array.prototype.map = function(callback, thisArg) {
    var mappedArr = []; 

    for(var i = 0; i < this.length; i++) {
        var mappedValue = callback.call(thisArg || global, this[i], i, this);
        mappedArr[i] = mappedValue;
    }

    return mappedArr; 
}

var testArr = [1, 2, 3].map(function(v, i) {
    return v * 2; 
}); 

console.log(testArr);
```

### this 주의하기

- 어떤 함수의 인자에 객체의 메서드를 전달하더라도 이는 결국 메서드가 아닌 함수일 뿐이다. 

```javascript
var obj = {
    vals: [1, 2, 3],
    logValues: function(v, i) {
        console.log(this, v, i);
    }
};

obj.logValues(1, 2); // obj

[4, 5, 6].forEach(obj.logValues); // window
```

### 콜백 지옥

- 콜백 지옥은 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상이다. 
- 들여쓰기 수준이 과도하게 깊어졌을뿐더러 값이 전달되는 순서가 '아래에서 위로' 향하고 있어 어색하게 느껴진다. 

```javascript
// 에스프레소 -> 아메리카노 -> 카페모카 -> 카페라떼

setTimeout(function(name) {
    console.log(name);

    setTimeout(function(name) {
        console.log(name);

        setTimeout(function(name) {
            console.log(name);

            setTimeout(function(name) {
                console.log(name);
                
            }, 500, '카페라떼'); 
        }, 500, '카페모카'); 
    }, 500, '아메리카노');
}, 500, '에스프레소'); 
```

### 콜백 지옥 해결 방법

1. 기명함수로 변환

- 일회성 함수를 전부 변수에 할당해야하는 문제 발생
- 코드명을 일일이 따라다녀야 하므로 오히려 헷갈릴 소지가 있다. 

```javascript
// 에스프레소 -> 아메리카노 -> 카페모카 -> 카페라떼

var addEspresso = function() {
    console.log('에스프레소');
    setTimeout(addAmericano, 500); 
}

var addAmericano = function() {
    console.log('아메리카노');
    setTimeout(addMocha, 500);
}


var addMocha = function() {
    console.log('카페모카');
    setTimeout(addLatte, 500); 
}

var addLatte = function() {
    console.log('카페라떼');
}

setTimeout(addEspresso, 500);  
```

2. Promise

- new 연산자와 함께 호출한 Promise의 인자로 넘겨주는 콜백 함수는 호출할 때 바로 실행되지만 그 내부에 resolve 또는 reject 함수를 호출하는 구문이 있을 경우 둘 중 하나가 실행되기 전까지는 다음(then) 또는 오류구문(catch)으로 넘어가지 않는다. 

```javascript
// 에스프레소 -> 아메리카노 -> 카페모카 -> 카페라떼

var addCoffee = function(name) {
    return function() {
        return new Promise(function(resolve) {
            setTimeout(function() {
                console.log(name);
                resolve(); 
            }, 500);  
        }); 
    }
}

addCoffee('에스프레소')()
    .then(addCoffee('아메리카노'))
    .then(addCoffee('카페모카'))
    .then(addCoffee('카페라떼'))

```

3. Promise + Async/await

- 비동기 작업을 수행하고자 하는 함수 앞에 async를 표기하고, 함수 내부에서 실질적인 비동기 작업이 필요한 위치마다 **await를 표기하는 것만으로 뒤의 내용을 Promise로 자동 전환**하고, 해당 내용이 resolve된 이후에야 다음으로 진행한다. 

💡 참고 자료

- [도서] 코어 자바스크립트 
- https://poiemaweb.com/




