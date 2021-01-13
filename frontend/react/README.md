# React

## Virtual DOM

- 일반적으로 DOM에 접근하여 여러번의 속성 변화, 여러번의 스타일 변화를 수행하면 그에따라 여러번의 Reflow, Repaint가 발생하게 됩니다. 하지만 Virtual DOM은 이렇게 변화가 일어나 Reflow, Repaint가 필요한 것들을 한번에 묶어서 dom에 전달하게 됩니다. 따라서 처리되는 Reflow, Repaint의 규모가 커질 수는 있지만 한번만 연산을 수행하게 됩니다. 이를 통해 여러번 Reflow, Repaint를 수행하며 연산이 반복적으로 일어나는 부분이 줄어들어 성능이 개선됩니다.
- 물론 프레임워크 없이 순수한 JavaScript로 똑같은 알고리즘을 구현할 수 있겠지만 실제로 구현하기에는 매우 어렵기 때문에 React, Angular가 이를 대신해주어 인기를 얻었다 생각합니다. (생산성 향상)

💡 참고 자료: https://velopert.com/3236
