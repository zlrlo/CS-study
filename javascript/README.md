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
'' == false; // true
null == false; // false
0 == "0" // true
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









 💡 참고 자료 
 - 부스트코스 웹 프로그래밍(풀스택) 강의<br>
 - https://webclub.tistory.com/1






