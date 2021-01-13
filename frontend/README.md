# Browser

## 브라우저는 어떻게 동작하는가?

### 브라우저 종류

- 인터넷 익스플로러, 파이어폭스, 사파리, 크롬, 오페라
- 파이어폭스, 크롬, 사파리는 오픈소스 브라우저이다.

### 브라우저의 주요 기능

- 브라우저의 주요 기능은 사용자가 선택한 자원을 서버에 요청하고 브라우저에 표시하는 것이다.
- 자원은 보틍 HTML문서지만 PDF나 이미지 또는 다른 형태일 수 있다.
- 자원의 주소는 URI에 의해 정해진다.

### 브라우저의 기본 구조

![image](https://user-images.githubusercontent.com/68647194/104270439-def37400-54db-11eb-962c-dc91fabf2532.png)

- `사용자 인터페이스`
  - 주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분이다.
- `브라우저 엔진`
  - 사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어
- `렌더링 엔진`
  - 요청한 콘텐츠를 표시. 예를 들어 HTML을 요청하면 HTML과 CSS를 파싱하여 화면에 표시한다.
- `통신`
  - HTTP 요청과 같은 네트워크 호출에 사용됨. 이것은 플랫폼 독립적인 인터페이스이고 각 플랫폼 하부에서 실행됨.
- `UI 백엔드`
  - 콤보 박스와 창 같은 기본적인 장치를 그림. 플랫폼에서 명시하지 않은 일반적인 인터페이스로서, OS 사용자 인터페이스 체계를 사용.
- `자바스크립트 해석기`
  - 자바스크립트 코드를 해석하고 실행
- `자료 저장소`
  - 쿠키를 저장하는 것과 같이 모든 종류의 자원을 하드 디스크에 자장할 필요가 있다.

> 크롬은 대부분의 브라우저와 달리 각 탭마다 별도의 렌더링 엔진 인스턴스를 유지하는 것이 주목할만하다. 각 탭은 독립된 프로세스로 처리된다.

### 렌더링 엔진

- 렌더링 엔진의 역할은 요청 받은 내용을 브라우저 화면에 표시하는 일이다.
- 종류 : 게코(Gecko)엔진, 웹킷(Webkit)엔진

### 웹킷 동작 과정

![image](https://user-images.githubusercontent.com/68647194/104271325-02b7b980-54de-11eb-85b9-5e97bae47a0f.png)

- 렌더링 엔진은 통신으로부터 요청한 문서의 내용을 얻는 것으로 시작하는데 문서의 내용은 보통 8KB 단위로 전송된다.
- HTML 파서는 HTML 마크업을 파싱 트리로 변환한다.
- 파싱 트리는 DOM 요소와 속성 노드의 트리로서 출력 트리가 된다.
- DOM (Document Object Model)
- DOM은 마크업과 1:1 관계를 맺는다.

```html
<html>
  <body>
    <p>Hello World</p>
    <div><img src="example.png" /></div>
  </body>
</html>
```

![image](https://user-images.githubusercontent.com/68647194/104272247-0f3d1180-54e0-11eb-9daf-a1055abc65bd.png)

#### 토큰화 알고리즘

![image](https://user-images.githubusercontent.com/68647194/104272593-cfc2f500-54e0-11eb-9b7f-472457120364.png)

```html
<html>
  <body>
    Hello world
  </body>
</html>
```

- 초기 상태는 자료 상태이다.
- '<' 문자를 만나면 상태는 태그 열림 상태로 변한다.
- a ~ z 까지의 문자를 만나면 시작 태그 토큰을 생성하고 상태는 태그 이름 상태로 변한다.
- '>' 를 만나면 새로운 토큰 이름을 붙이고 발행한다. ex) html 토큰, body 토큰
- 문자를 만나면 문자 토큰이 생성되고 발행된다. 종료 태그의 '<' 문자를 만날 때 까지 진행된다.
- '/' 문자는 종료 태그 토큰을 생성하고 '>' 문자를 만날 때 까지 태그 이름 상태를 유지한다.

![image](https://user-images.githubusercontent.com/68647194/104273979-30533180-54e3-11eb-9003-2b39e8f7f3e2.png)

#### 트리 구축 알고리즘

```html
<html>
  <body>
    Hello world
  </body>
</html>
```

- 파서가 생성되면 문서 객체가 생성된다. (트리의 최상위 객체는 문서이다.)
- 트리 구축 단계의 입력 값은 토큰화 단계에서 만들어지는 일련의 토큰이다.
- html 토큰을 받으면 HTMLHtmlElement 요소를 생성하고 문서 객체의 최상단에 추가된다.
- head 토큰을 받으면 HTMLHeadElement 요소를 생성하고 트리에 추가한다.<br>
  (head 토큰이 없더라고 묵시적으로 생성되어 트리에 추가될 것이다.)
- head 종료 토큰을 받으면 head 다음 모드가 된다.
- body 토큰이 처리되어 HTMLBodyElement가 생성되어 추가된다.
- "Hello world" 문자열은 첫 번째 토큰이 생성되고 "본문" 노드가 추가 되면서 다른 문자들이 그 노드에 추가된다.
- body 종료 토큰을 받으면 body 다음 모드가 된다.
- html 종료 태그를 만나고, 마지막 파일 토큰을 받으면 파싱을 종료한다.

#### 웹킷 CSS 파서

![image](https://user-images.githubusercontent.com/68647194/104284445-60a3cb80-54f5-11eb-80fe-41cb1927e58c.png)

#### 스크립트와 스타일 시트의 진행 순서

- 스크립트

  - 웹은 파싱과 실행이 동시에 수행되는 동기화 모델이다.
  - 제작자는 <script> 태그를 만나면 즉시 파싱하고 실행하기를 기대한다.
  - 스크립트가 실행되는 동안 문서의 파싱은 중단된다.
  - 스크립트가 외부에 있는 경우 우선 네트워크로부터 자원을 가져와야 하는데 이 또한 실시간으로 처리되고 자원을 받을 때 까지 파싱은 중단된다.
  - 제작자는 스크립트를 지연으로 표시할 수 있는데 지연으로 표시하게 되면 문서 파싱은 중단되지 않고 문서 파싱이 완료된 이후에 스크립트가 실행된다.
  - HTML5는 스크립트를 비동기로 처리하는 속성을 추가했기 때문에 별도의 맥락에 의해 파싱되고 실행된다.

- 예측 파싱

  - 웹킷과 파이어폭스는 예측 파싱과 같은 최적화를 지원한다.
  - 스크립트를 실행하는 동안 다른 스레드는 네트워크로부터 다른 자원을 찾아 내려받고 문서의 나머지 부분을 파싱한다.
  - 이런 방법은 자원을 병렬로 연결하여 받을 수 있고 전체적인 속도를 개선한다.
  - 참고로 예측 파서는 DOM 트리를 수정하지 않고 메인 파서의 일로 넘긴다. 예측 파서는 외부 스크립트, 외부 스타일 시트와 외부 이미지와 같이 참조된 외부 자원을 파싱할 뿐이다.

- 스타일 시트
  - 이론적으로 스타일 시트는 DOM 트리를 변경하지 않기 때문에 문서 파싱을 기다리거나 중단할 이유가 없다. 그러나 스크립트가 문서를 파싱하는 동안 스타일 정보를 요청하는 경우라면 문제가 된다. 스타일이 파싱되지 않은 상태라면 스크립트는 잘못된 결과를 내놓기 때문에 많은 문제를 야기한다. 파이어폭스는 아직 로드 중이거나 파싱 중인 스타일 시트가 있는 경우 모든 스크립트의 실행을 중단한다. 한편 웹킷은 로드되지 않은 스타일 시트 가운데 문제가 될만한 속성이 있을 때에만 스크립트를 중단한다.

#### 렌더 트리 구축

- DOM 트리가 구축되는 동안 브라우저는 렌더 트리를 구축한다.
- 이 구성 요소를 웹킷에서는 렌더러라는 용어를 사용한다.
- 렌더러는 자신과 자식 요소를 어떻게 배치하고 그려내야 하는지 알고 있다.
- 각 렌더러는 CSS2명세에 따라 노드의 CSS박스에 부합하는 사각형을 표시한다.
- 웹킷은 dispaly 속성에 따라 DOM 노드에 어떤 유형의 렌더러를 만들어야 하는지 결정한다.
- 요소 유형 또한 고려해야 하는데, 예를 들면 폼 콘드롤과 표는 특별한 구조이다. 요소가 특별한 렌더러를 만들어야 한다면 웹킷은 createRenderer 메서드를 무시하고 비기하학 정보를 포함하는 스타일 객체를 표한다.

#### DOM 트리와 렌더 트리의 관계

- 렌더러는 DOM 요소에 부합하지만 1:1로 대응하는 관계는 아니다.
- 예를 들어 head 요소와 같은 **비시각적 DOM 요소는 렌터 트리에 추가되지 않는다.**
- 또한 display 속성에 none 값이 할당된 요소는 트리에 나타나지 않는다.
  (visibiliy 속성에 hidden 값이 할당된 요소는 트리에 나타난다.)
- 여러 개의 시각 객체와 대응하는 DOM 요소도 있는데 이것들은 보통 하나의 사각형으로 묘사할 수 없는 복잡한 구조다.
- 예를 들면 select 요소는 '표시 영역, 드롭다운 목록, 버튼' 표시를 위한 3개의 렌더러가 있다. 또한 한 줄에 충분히 표시할 수 없는 문자가 여러 줄로 바뀔 때 새 줄은 별도의 렌더러로 추가된다.
- 어떤 렌더 객체는 DOM 노드에 대응하지만 트리의 동일한 위치에 있지 않다. float 처리된 요소 또는 position 속성 값이 absolute로 처리된 요소는 흐름에서 벗어나 트리의 다른 곳에 배치된 상태로 형상이 그려진다. 대신 자리 표시자가 원래 있어야 할 곳에 배치된다.

#### 트리를 구축하는 과정

- 웹킷에서는 스타일을 결정하고 렌더러를 만드는 과정을 "어태지먼드(ataachment)" 라고 부른다.
- 모든 DOM 노드에는 attach 메서드가 있다.
- 어태치먼트는 동기적인데 DOM 트리에 노드를 추가하면 새 노드의 "attach" 메서드를 호출한다.
- html 태그와 body 태그를 처리함으로써 렌더 트리 루트를 구성한다. 루트 렌더 객체는 CSS 명세에서 포함 블록(다른 모든 블록을 포함하는 최상위 블록)이라고 부르는 그것과 일치한다. 파이어폭스는 이것을 ViewPortFrame이라 부르고 웹킷은 RenderView라고 부른다. 이것이 문서가 가리키는 렌더 객체다. 트리의 나머지 부분은 DOM 노드를 추가함으로써 구축된다.

![image](https://user-images.githubusercontent.com/68647194/104288482-32c18580-54fb-11eb-88a6-d5a9a2171ff1.png)

💡 참고 자료: https://d2.naver.com/helloworld/59361