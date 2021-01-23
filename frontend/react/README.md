# React

## React 장점

- 재사용 가능한 컴포넌트를 만들 수 있다.
  - UI 일부가 여러 번 사용되거나, UI 일부가 자체적으로 복잡한 경우

## Virtual DOM

- Virtual DOM은 실제 DOM의 구조와 비슷한, `React 객체의 트리`다.
- **개발자는 직접 DOM을 제어하지 않고 Virtual DOM을 제어하고, React에서 적절하게 Virtual DOM을 DOM에 반영하는 작업을 한다.**

## JSX

- JavaScript를 확장한 문법이다.
- React Element를 생성한다.
- JSX 문법은 React.createElement()를 호출하는 편리한 문법에 불과하다.
- 즉, JSX 문법은 React.createElement()를 호출하기 위한 하나의 방법일 뿐이고 Babel(Javascript 트랜스파일러)을 통해 파싱되고 트랜스 파일링된다.

```javascript
ReactDom.render(
    <App/>,
    document.getElementById('root');
)
```

- 앞의 코드의 JSX 문법을 변환하면 다음과 같은 JavaScript코드가 만들어진다.

```javascript
ReactDom.render(
    React.createElement(App),
    document.getElementById('root');
)
```

## React.createElement

- ReactElement는 내부적으로 다음 값을 가지는 `Object`이다.
- 화면에 표시하려는 항목에 대한 설명이라고 생각할 수 있다.
- 엘리먼트는 React 앱의 가장 작은 단위이다.
- 리액트에서는 엘리먼트가 모여 컴포넌트라는 구조를 만든다.

```javascript
// 주의: 다음 구조는 단순화되었습니다
const element = {
  type: "h1",
  props: {
    className: "greeting",
    children: "Hello, world!",
  },
};
```

## React.Component

- 리액트에서 재사용 할 수 있는 UI를 하나의 묶음으로 컴포넌트라고 한다.
- 즉, 엘리먼트의 조각들을 모아서 모듈화 한것을 의미한다.
- 리액트 컴포넌트에는 Functional Component와 Class Component 두 종류가 있다.

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

1. `<Welcome name="Sara" />` 엘리먼트로 ReactDOM.render()를 호출한다.
2. React는 {name: 'Sara'}를 props로 하여 Welcome 컴포넌트를 호출한다.
3. Welcome 컴포넌트는 결과적으로 `<h1>Hello, Sara</h1>` 엘리먼트를 반환한다.
4. React DOM은 `<h1>Hello, Sara</h1>` 엘리먼트와 일치하도록 DOM을 효율적으로 업데이트한다.

## ReactDOM.render

- React 컴포넌트를 웹 브라우저에 렌더링 하는 API
- 실제로 대부분의 React 앱은 ReactDom.render()를 한 번만 호출한다.

## Props, State

- 컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달할 수 있다.
- 자식 컴포넌트는 props로 받은 것이 어디서 왔는지, 수동으로 입력한 것인지 알지 못한다.
- 일반적으로 이를 "하향식" 또는 "단방향식" 데이터 흐름이라고 한다.
- 모든 state는 항상 특정한 컴포넌트가 소유하고 있으며, 그 state로 파생된 UI 또는 데이터는 오직 트리구조에서 자신의 "아래"에 있는 컴포넌트에만 영향을 미친다.

### Props

- Props는 읽기 전용이다.
- 순수 함수처럼 동작해야한다. (입력(props)를 변경X)

### State

- State는 Props와 유사하지만, 비공개이며 컴포넌트에 의해 완전히 제어됩니다.
- this.state를 지정할 수 있는 유일한 공간은 constructor이다.
  (즉, 직접 수정이 불가능 하다. ex) this.state.comment = 'Hello')

### setState의 비동기 처리

- state의 변경이 한꺼번에 여러번 많이 발생하게 된다면 그 만큼 렌더링이 많이 발생하게 된다.
- 이를 방지하기 위해 state의 변경사항을 즉시 반영하지 않고, 변경사항을 대기열에 넣어 한꺼번에 적용시킨다.
- 이 과정에서 setState 호출 후의 state 값이 개발자가 의도하지 않는 값이 될 수도 있다.
- setState는 이벤트 핸들러 내에서 비동기적으로 동작한다.
- 즉, 하나의 **이벤트 핸들러 내**에서 setState가 여러번 호출된다면, **이벤트가 끝난 시점에 state를 일괄적으로 업데이트**하고 렌더링한다.

- 일괄 처리 예시

```javascritp
  Object.assign(
    previousState,
    {quantity: state.quantity + 1},
    {quantity: state.quantity + 1},
    ...
  )
```

## React 작동 방법

- 직접 구현해보기<br>
  - https://enro2414-40667.medium.com/virtual-dom-%EB%B2%84%EC%B6%94%EC%96%BC-%EB%8F%94-%EA%B0%80%EC%83%81-%EB%8F%94-%EC%9D%84-%EC%A7%81%EC%A0%91-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EC%9E%90-1c44606ea9b1
  - https://wonism.github.io/deep-dive-into-vdom/

## 비제어 컴포넌트 (Ref 사용)

- 비제어 컴포넌트를 사용할 때 React와 non-React 코드를 통합하는 것이 쉬울 수 있다.

## 리액트 라이프사이클

### class component

![image](https://user-images.githubusercontent.com/68647194/105005671-f94bc580-5a78-11eb-8698-b74d966df54a.png)

### Functional Component (Hook)

- Hook : 함수 컴포넌트에서 React state와 생명주기 기능을 연동할 수 있게 해주는 함수이다.
- Hook은 state 그 자체가 아니라, 상태 관련 로직을 재사용하는 방법이다.
- Custom Hook은 기능이라기보다는 컨벤션에 가깝다. 이름이 use로 시작하고, 안에서 다른 Hook을 호출한다면 그 함수를 custom Hook이라고 부를 수 있다.
- useState()
  - 현재의 state 값과 이 값을 업데이트하는 함수를 쌍으로 제공한다.
- useEffect()

  - React는 매 렌더링 이후에 effects를 실행한다. 리액트는 efftect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장한다.

### Custom Hook

- Hook을 이용하면 생명주기 메서드에 따라서가 아니라 코드가 무엇을 하는지에 따라 나눌 수 있다.
- 리액트는 컴포넌트에 사용된 모든 effect를 지정된 순서에 맞춰 적용한다.

## 불변성

- 불변성을 지켜주지 않으면 비교 불가능

💡 참고 자료:

- https://velopert.com/3236
- https://velog.io/@cada/React%EC%9D%98-setState%EA%B0%80-%EC%9E%98%EB%AA%BB%EB%90%9C-%EA%B0%92%EC%9D%84-%EC%A3%BC%EB%8A%94-%EC%9D%B4%EC%9C%A0
- https://ko.reactjs.org/docs/hello-world.html
