## Rest/Spread 프로퍼티

```javascript
// 객체 리터럴 Rest/Spread 프로퍼티
// Spread 프로퍼티
const n = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }

// Rest 프로퍼티
const { x, y, ...z } = n;
console.log(x, y, z); // 1 2 { a: 3, b: 4 }

// spread 문법
[x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [ 2, 3 ]
```

### 중첩 객체

```jsx
const person = {
  name: 'Lee',
  address: {
    zipCode: '03068',
    city: 'Seoul'
  }
};

const { address: { city } } = person;
console.log(city); // 'Seoul'
```