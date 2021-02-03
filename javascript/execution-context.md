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

### LexicalEnvironment

```
environmentRecord (snapshot)
outerEnvironmentReference (snapshot)
```

- 사전적 환경
- 컨텍스트를 구성하는 환경 정보들을 사전에서 접하는 느낌으로 모아놓은 것







💡 참고 자료

- [도서] 코어 자바스크립트 




