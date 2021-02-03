## `undefined` 와 `null` 의 차이점

- 자바스크립트에는 '없음'을 나타내는 값이 두 가지가 있다. 

- `undefined`

  - 사용자가 명시적으로 지정할 수도 있지만 값이 존재하지 않을 때 **자바스크립트 엔진이 자동으로 부여한다.**
  - 다음 세 경우가 이에 해당한다. 
    1. 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때 
    2. 객체 내부에 존재하지 않는 프로퍼티에 접근하려고 할 때
    3. return 문이 없거나 호출되지 않는 함수의 실행 결과

  ```javascript
  var hello;
  console.log(typeof hello); // undefined
  ```

  ### '비어있는 요소'와 'undefined'를 할당한 요소는 다르다. 

  - 비어있는 요소는 순회와 관련된 많은 메서드들의 순회 대상에서 제외됩니다. 
  - 배열에서 값이 지정되지 않은 인덱스는 '아직은 존재하지 않는 프로퍼티'에 지나지 않다. 

  ```javascript
    var arr1 = [undefined, 1];

    var arr2 = []; 
    arr2[1] = 1; 

    // undefined 1
    arr1.forEach(function(v, i) {
        console.log(v);
    }); 

    // 1
    arr2.forEach(function(v) {
        console.log(v);
    }); 
  ```

  > 자바스크립트 엔진이 반환하는 경우는 우리의 통제 범위를 벗어나므로 모든 undefined가 오직 이 경우만 해당하게끔 해준다. 다시 말해 직접 undefined를 할당하지 않으면 혼란을 피할 수 있다. 

- `null`

  - 주로 **객체**를 담을 변수를 초기화할 때 많이 사용한다.
  - **typeof null은 object**
  - 따라서 어떤 변수의 값이 null인지 여부를 판별하기 위해서는 typeof 대신 다른 방식으로 접근해야 한다. 

  ```javascript
  var data1 = 0; //Number 변수 초기화
  var data2 = ""; // String 변수 초기화
  var data3 = false; // Boolean 변수 초기화
  var data4 = null; // Object 변수 초기화
  ```


#### 참고

- 동등 연산자(==)로 비교할 경우 null과 undefined가 서로 같다고 판단한다. 

```javascript
console.log(null == undefined); // true
```

💡 참고 자료

- [도서] 코어 자바스크립트 




