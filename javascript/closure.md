## 클로저

- 클로저는 여러 함수형 프로그래밍 언어에서 등장하는 보편적인 특성이다. 
- 자바스크립트 고유의 개념이 아니다. 
- **어떤 함수에서 선언한 변수를 참조하는 내부함수에서만 발생하는 현상이다.**
- **외부 함수의 LexicalEnvironment가 가비지 컬렉팅되지 않는 현상이다.** 


### 클로저 이해하기

```javascript
var outer = function() {
    var a = 1; 
    var inner = function() {
        console.log(++a);
    };

    inner(); 
}

outer(); 
```

- outer함수의 실행 컨텍스트가 종료되면 LexicalEnvironment에 저장된 식별자들(a, inner)에 대한 참조를 지운다. 
- 각 주소에 저장돼 있던 값들은 자신을 참조하는 변수가 하나도 없게 되므로 가비지 컬렉터의 수집 대상이 된다. 

```javascript
var outer = function() {
    var a = 1; 
    var inner = function() {
        return ++a; 
    }

    return inner; 
}

var outer2 = outer(); 
console.log(outer2()); // 2
console.log(outer2()); // 3
```

- inner 함수의 실행 컨택스트 outerEnvironmentReference에는 inner 함수가 선언된 위치의 LexicalEnvironment가 참조 복사된다. 
- 외부 함수인 outer의 실행이 종료되더라도 내부 함수인 inner 함수는 언제가 outer2를 실행함으로써 호출될 가능성이 열렸다. 
- 언젠가 inner 함수의 실행 컨텍스트가 활성화되면 outerEnvironmentReference가 outer 함수의 LexivalEnvironment를 필요로 할 것이므로 **가비지 컬렉터의 수집 대상에서 제외 된다.**
- 따라서 inner 함수의 실행 시점에는 outer 함수는 이미 실행이 종료된 상태임에도 변수에 접근할 수 있다.  

### 클로저와 메모리 관리

- '메모리 누수'는 개발자의 의도와 달리 어떤 값이 참조 카운트가 0이 되지 않아 가비지 컬렉터의 수거 대상이 되지 않는 경우이다. 
- 참조 카운트를 0으로 만드는 방법은? 
    - 보통 null이나 undefined를 할당하면 된다. 

### 클로저 활용 사례

- 고차 함수 활용

```javascript
var alertFruitBuilder = function(fruit) {
    return function() {
        alert('your choice is' + fruit);
    }
}

fruits.forEach(function(fruit) {
    var $li = document.createElement('li');
    $li.innerText = fruit;
    $li.addEventListener('click', alertFruitBuilder(fruit));
    $ul.appendChild($li);
}); 
```

- 정보 은닉
    - 비공개 변수를 만들 수 있다.
    - 프로그램 사용자는 공개한 메소드만 사용한다.
    - 사용자를 통제하기 위한 기본적인 방법이 바로 클로저이다.

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

- 부분 적용 함수
    - 부분 적용 함수란 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가, 나중에 (n-m)개의 인자를 넘기면 비로소 원래 함수의 실행 결과를 얻을 수 있게끔 하는 함수이다. 
    - bind 메서드의 실행 결과가 바로 부분 적용 함수이다. 
    - 미리 일부 인자를 넘겨두어 기억하게끔 하고 추후 필요한 시점에 기억했던 인자들까지 함께 실행하게 한다는 개념 자체가 클로저의 정의에 정확히 부합한다. 
    - **디바운스**는 짧은 시간 동안 동일한 이벤트가 많이 발생할 경우 이를 전부 처리하지 않고 처음 또는 마지막에 발생한 이벤트에 대해 한번만 처리하는 것으로, 프론트엔드 성능 최적화에 큰 도움을 주는 기능이다.

- 커링 함수
    - 커링 함수란 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출될 수 있게 체인 형태로 구성한 것을 말한다. 
    - 커링은 한 번에 하나의 인자만 전달하는 것을 원칙으로 한다. 
    - **원하는 시점까지 지연시켰다가 실행하는 것이 요긴한 상황이라면 커링을 쓰기에 적합하다.** 
    - **혹은 프로젝트 내에서 자주 쓰이는 함수의 매개변수가 항상 비슷하고 일부만 바뀌는 경우에도 적절한 후보가 된다.** 

    ```javascript
    // 10이랑 비교해서 큰 값 찾기
    // 인자가 많아질수록 가독성이 떨어진다는 단점이 있다. 
    // var curry3 = func => a => b => func(a, b);
    var curry3 = function(func) {
        return function(a) {
            return function(b) {
                return func(a, b);
            }
        }
    }

    var getMaxWith10 = curry3(Math.max)(10); 
    console.log(getMaxWidth10(8)); // 10
    console.log(getMaxWidth10(25)); // 25
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

💡 참고 자료

- [도서] 코어 자바스크립트 

