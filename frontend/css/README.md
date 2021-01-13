# CSS

## css 스타일 우선순위

1. 속성값 뒤에 !important
2. 태그에 inlin으로 style 속성 지정
3. 선택자가 #id
4. 선택자가 .class 및 pseudo 클래스(:hover 같은 것)
5. 선택자가 tag 이름

- 스타일에 적용되어진 조합들을 토대로 특정도 값을 계산해서 가장 많은 점수를 받은 것이 우선된다.

### 특정도 계산식

- 인라인 스타일 1000점
- ID 선택자 100점
- 클래스 선택자 및 가상 클래스(:before)속성 10점
- 태그 선택자 1점

> 점수마저 같다면 태그 같은 경우에는 나중에 나온것이, class나 id 같은 경우에는 태그에 먼저 나온 것이 우선순위를 가진다.

### Reflow, Repaint

- Layout : Render Tree 노드들이 가지고 있는 스타일과 속성에 따라서 브라우저 화면의 어느위치에 어느크기로 출력될지 `계산하는 단계`

- Paint : Layout 계산이 완료되면 각 요소들을 `실제 화면에 그리는 단계`.

#### Reflow

- 어떠한 액션이나 이벤트에 따라 html 요소의 크기나 위치 등 레이아웃 수치를 수정하면 그에 영향을 받는 자식 노드나 부모 노드를 포함하여 Layout 과정을 다시 수행하게 된다. 이렇게 되면 Render Tree와 각 요소들의 크기와 위치를 다시 계산하게 된다. 이러한 과정을 Reflow라고 한다.
- Reflow가 일어아는 대표적인 경우
  - 페이지 초기 렌더링 시 (최초 Layout 과정)
  - 윈도우 리사이징 시 (Viewport 크기 변경시)
  - 노드 추가 또는 제거
  - 요소의 위치, 크기 변경(left, top, margin, padding, border, width, height, 등...)
  - 폰트 변경(텍스트 내용)과 이미지 크기 변경(크기가 다른 이미지로 변경 시)

#### Repaint

- Reflow만 수행되면 실제 화면에 반영되지 않는다.
- 결국 Paint 단계가 다시 수행되는 것이며 이를 Repaint라고 한다.
- 하지만 무조건 Reflow가 일어나야 Repaint가 일어나는 것은 아니다.
- background-color, visibility와 같이 레이아웃에는 영향을 주지 않는 스타일 속성이 변경되었을 때는 Reflow를 수행할 필요가 없기 때문에 Reapint만 수행하게 된다.

#### Reflow, Repaint 줄이기

1. 사용하지 않는 노드에는 visibilty: invisible 보다 display: none을 사용한다.
2. Reflow, Repaint가 발생하는 속성 피하기
   - Reflow가 일어나면 Repaint는 필연적으로 일어나야 하기 때문에 가능하다면 Reflow가 발생하는 속성보다 Repaint만 발생하는 속성을 사용하는 것이 좋다.
   - transform, opacity와 같은 속석은 Reflow, Repaint가 일어나지 않는다.
3. 영향을 주는 노드 줄이기
   - JavaScript, CSS를 조합하여 애니메이션이 많거나 레이아웃 변화가 많은 요소의 경우 position을 absolute 또는 fixed를 사용하여 영향을 받는 주변 노드들을 줄일 수 있다. fixed와 같이 영향을 받는 노드가 전혀 없는 경우 reflow과정이 전혀 필요가 없어지기 때문에 Repaint연산 비용만 들게 된다.
4. 프레임 줄이기
   - 단순히 생각하면 0.1초에 1px씩 이동하는 요소보다 3px씩 이동하는 요소가 Reflow, Repaint 연산비용이 3배가 줄어든다고 볼 수 있다. 따라서 부드러운 효과를 조금 줄여 성능을 개선할 수 있다.

## Position

`static`

- 일반 모든 태그들은 처음에 position: static 상태이다.
- 차례대로 왼쪽에서 오른쪽, 위에서 아래로 쌓인다.

`relative`

- 태그의 위치를 살짝 변경하고 싶을 때, position: relative를 사용한다.
- top, left, right, bottom 속성을 사용해 위치 조절이 가능하다.
  ``

`absolute`

- relative가 static인 상태를 기준으로 주어진 픽셀만큼 움직였다면,
- absolute는 position: `static 속성을 가지고 있지 않은 부모`를 기준으로 움직인다.
- 만약 부모 중에 포지션이 relative, absolute, fixed인 태그가 없다면 가장 위에 태그(body)가 기준이 된다.

> absolute가 되면 div여도 더는 width: 100%가 아니다.

`fixed`

- 스크롤을 내려도 그 자리에 계속있다.

> fixed도 absolute 처럼 더는 div가 width: 100%가 아니다.

💡 참고 자료

- https://boxfoxs.tistory.com/408
- https://www.zerocho.com/category/CSS/post/5864f3b59f1dc000182d3ea1
