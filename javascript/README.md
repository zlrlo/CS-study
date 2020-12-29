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






 💡 참고 문서: 부스트코스 웹 프로그래밍(풀스택) 강의






