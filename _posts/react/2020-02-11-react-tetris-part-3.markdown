---
layout: post
type:   posts
title:  "[3] React 기반 테트리스 만들기 - Model편"
date:   "2020-02-11"
categories:
  - "React Tetris 튜토리얼"
tags:
  - react
  - game
  - development
  - tutorial
  - react-tetris
---

## Redux Store 기본 세팅

거두절미하고, 우선은 기본적인 Redux store를 만들어주어야 한다.

**src/store/index.js**
{% highlight javascript %}
import { createStore } from 'redux';
import rootReducer from './reducer';

const store = createStore(rootReducer);
export default store;
{% endhighlight %}

**src/store/reducer/index.js**
{% highlight javascript %}
TetriminoModel: {
    tetrimino_type: 0,
    tetrimino_position: [0, 0],
    tetrimino_color: "#000000"
},
NextModel: [], // just stores list of tetrimino_type
GameBoardModel: {
    board_size: [0, 0],
    board_status: [] // in form of: [[[x,y,color], [x,y,color]], [[x,y,color], [x,y,color]], ...]
}

function rootReducer(state = initialState, action) {
    return state;
};

export default rootReducer;
{% endhighlight %}

우리가 이전 포스팅에서 만들었던 그 구조를 기억하며 store의 초기값을 설정해주었다. 아직 이름만 지어주고 value는 넣지 않았지만, 추후에 action을 사용하여 넣어주도록 할 것이다.

{% include adsense.html %}

## 기본적인 Action 만들기

이제 우리의 reducer에 다양한 action 함수들을 생성해주도록 한다.

**src/store/actions/action-types.js**
{% highlight javascript %}
export const UPDATE_TETRIMINO = "tetris/UPDATE_TETRIMINO";
export const UPDATE_GAMEBOARD = "tetris/UPDATE_GAMEBOARD";
export const UPDATE_NEXT = "tetris/UPDATE_NEXT";
{% endhighlight %}

**src/store/actions/index.js**
{% highlight javascript %}
import { UPDATE_TETRIMINO, UPDATE_GAMEBOARD, UPDATE_NEXT } from "./action-types";

export function updateTetrimino(payload) {
    return { type: UPDATE_TETRIMINO, payload }
};
export function updateGameboard(payload) {
    return { type: UPDATE_GAMEBOARD, payload }
};
export function updateNext(payload) {
    return { type: UPDATE_NEXT, payload }
};
{% endhighlight %}

각각의 action 함수는 단순하게 각 모델을 업데이트해주는 역할을 수행한다. 세부적인 action들은 추후에 필요하면 추가해주도록 하고 우선은 이렇게 두기로 한다.

이제 redux store를 action을 활용하도록 업데이트해야 한다. 위에 만들어둔 store에 rootReducer 부분만 수정하면 된다.

**src/store/reducer/index.js**
{% highlight javascript %}
...
function rootReducer(state = initialState, action) {
    if(action.type === UPDATE_TETRIMINO) {
        state.tetriminoModel = action.payload;
    }
    if(action.type === UPDATE_GAMEBOARD) {
        state.GameBoardModel = action.payload;
    }
    if(action.type === UPDATE_NEXT) {
        state.NextModel = action.payload;
    }
    return state;
};
...
{% endhighlight %}

단순하게 payload로 들어온 오브젝트를 state에 업데이트해주도록만 설정해 두었다. 추후에 다양한 Action을 추가하여 보기 좋게 다듬어야 할 것이다.

### 간단한 모델링은 끝!
기본적인 모델은 완성해두었다. 이제 다음 포스팅에서는 이 모델을 화면에 표시할 View를 만들도록 할 것이다. 별다른 Image resource를 사용하지 않고 기본적인 HTML5+CSS3를 통하여 만들 것이기 때문에 HTML5와 CSS3에 흥미가 있는 독자들은 재밌게 읽을 수 있을 것이라고 생각한다.



<!--
Code Highlight
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}
-->