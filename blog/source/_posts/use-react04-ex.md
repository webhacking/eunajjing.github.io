---
title: 리액트를 다루는 기술04 (실습)
date: 2019-02-27 10:06:18
tags:
categories:
- 개발공부
- React
---

# 리덕스 사용해보기

`yarn add redux react-redux`로 의존 설정을 더해준다.

함수형 컴포넌트의 경우 이전 기초 예제들과 거의 유사하다.

```javascript
import React from 'react';

const Counter = ({number, color, onIncrement, onDecrement, onSetColor}) => {
    return (
        <div className="Counter" onClick={onIncrement} onContextMenu={(e) => {
            // onContextMenu : 오른쪽 버튼 클릭 시의 이벤트
            e.preventDefault();
            // 해당 이벤트를 막는다
            onDecrement();
        }}
        onDoubleClick={onSetColor} style={{backgroundColor: color}}>{number}</div>
    );
};

Counter.defaultProps = {
    number: 0,
    color: 'black',
    onIncrement: () => console.warn("증가 아직 안됨"),
    onDecrement: () => console.warn("감소 아직 안됨"),
    onSetColor: () => console.warn("컬러 변경 아직 안됨")
}

export default Counter;
```

## 액션

### 액션 정의

```javascript
// actionType.js

export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';
export const SET_COLOR = 'SET_COLOR';
```

### 액션 생성 메서드

```javascript
// action.js

import * as types from './ActionTypes';

export const increment = () => ({
    type: INCREMENT
});
export const decrement = () => ({
    type: DECREMENT
});
export const setColor = (color) => ({
    type: SET_COLOR,
    color
});

// color의 경우 객체 자체를 넣는다.
```

## 리듀서

```javascript
import * as types from '../actions/ActionTypes';

// 초기값 설정
const initialState = {
    color: 'black',
    number: 0
}

function counter(state=initialState, action) {
    switch (action.type) {
        case types.INCREMENT:
        return {
            ...state,
            number: state.number+1
        };
        case types.DECREMENT:
        return {
            ...state,
            number: state.number-1
        };
        case types.SET_COLOR:
        return {
            ...state,
            color: action.color
        };
        default:
        return state;
    }
}

export default counter;
```

## 관련 라이브러리 사용

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './containers/App';
import {createStore} from 'redux';
import reducers from './reducers';
import {Provider} from 'react-redux';

const store = createStore(reducers);

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root'));
```

- `store`를 생성하며 아까 만든 `reduces`를 파라미터로 넣어준다.
- 최상위 컴포넌트를 `Provider`로 감싸주고 props로 `store`를 보낸다.

## 컨테이너 생성

```javascript
import Counter from '../components/Counter';
// 함수형 컴포넌트를 받아와서 하단의 커넥트로 묶어줌
import * as actions from '../actions';
import {connect} from 'react-redux';

export function getRandomColor() {
    const color=[
        '#495057',
        ...
    ];

    const random = Math.floor(Math.random() * 13);

    return color[random];
}

// connect 메서드에 들어가는 파라미터 함수로,
// 옵션이며 현재 스토어의 상태를 받아 컴포넌트의 props로 사용할 객체 반환
const mapStateToProps = (state) => ({
    color: state.color,
    number: state.number
});

// 액션 생성 메서드를 사용해 액션 생성 후
// 해당 액션을 디스패치하는 메서드를 만든 후 이를 props로 연결
const mapDispatchToProps = (dispatch) => ({
    onIncrement: () => dispatch(actions.increment()),
    onDecrement: () => dispatch(actions.decrement()),
    onSetColor: () => {
        const color = getRandomColor();
        dispatch(actions.setColor(color));
    }
});

// 뷰단 컴포넌트를 애플리케이션의 데이터 레이어와 묶는 역할을 한다
const CounterContainer = connect(
    mapStateToProps, mapDispatchToProps
)(Counter);

export default CounterContainer;
```

## 최상위 컴포넌트에서 컨테이너 받아오기

```javascript
import React, { Component } from 'react';
import CounterContainer from './CounterContainer';

class App extends Component {
    render() {
        return (
            <div>
                <CounterContainer />
            </div>
        );
    }
}

export default App;
```

# 서브 리듀서 사용해보기

위의 코드와 거의 비슷한데, 일단 리듀서들을 두 개 작성한다.

```javascript
// number.js
import * as types from '../actions/ActionTypes';

const initialState = {
    number: 0
};

const number = (state=initialState, action) => {
    switch (action.type) {
        case types.INCREMENT:
        return {
            number: state.number+1
        };
        case types.DECREMENT:
        return {
            number: state.number-1
        };
        default:
        return state;
    }
}

export default number;
```

그리고 통합 리듀서에서 이들을 합친다.

```javascript
import color from './color';
import number from './number';
import {combineReducers} from 'redux';
// 서브 리듀서들을 하나로 합치는 라이브러리

const reducers = combineReducers({
    numberData : number,
    colorData : color
});

export default reducers;
```

컨테이너에서 state 참조는 이렇게 한다.

```react
//CounterContainer.js

const mapStateToProps = (state) => ({
    color: state.colorData.color,
    number: state.numberData.number
});
```

# list 형태 데이터 사용

액션 생성 함수에 인덱스를 넣는다

```javascript
// index.js

import * as types from './ActionTypes';
// 액션 타입 정의를 받아온다

export const create = (color) => ({
    type: types.CREATE,
    color
});

export const remove = () => ({
    type: types.REMOVE
});

export const increment = (index) => ({
    type: types.INCREMENT,
    index
});
export const decrement = (index) => ({
    type: types.DECREMENT,
    index
});
export const setColor = ({color, index}) => ({
    type: types.SET_COLOR,
    color,
    index
});
```

리듀서가 복잡해진다. 가독성이 떨어지므로 추후 다른 것을 이용해 대체한다.

```javascript
// index.js

import * as types from '../actions/ActionTypes';

const initaialState = {
    counters: [
        {
            color: 'black',
            number: 0
        }
    ]
};

function counter(state = initaialState, action) {
    const {counters} = state;
    switch(action.type) {
        case types.CREATE:
        return {
            counters: [
                ...counters,
                {
                    color: action.color,
                    number: 0
                }
            ]
        };
        case types.REMOVE:
        return {
            counters : counters.slice(0, counters.length - 1)
        };
        case types.INCREMENT:
        return {
            counters: [
                ...counters.slice(0, action.index),
                {
                    ...counters[action.index],
                    number: counters[action.index].number + 1
                },
                ...counters.slice(action.index+1, counters.length)
            ]
        };
        case types.DECREMENT:
        return {
            counters: [
                ...counters.slice(0, action.index),
                {
                    ...counters[action.index],
                    number: counters[action.index].number - 1
                },
                ...counters.slice(action.index+1, counters.length)
            ]
        };
        case types.SET_COLOR:
        return {
            counters: [
                ...counters.slice(0, action.index),
                {
                    ...counters[action.index],
                    color : action.color
                },
                ...counters.slice(action.index+1, counters.length)
            ]
        };
        default:
        return state;
    }
}

export default counter;
```

## 함수형 컴포넌트

```javascript
// Buttons.js
import React from 'react';

const Buttons = ({onCreate, onRemove}) => {
    return (
        <div className="Buttons">
            <div className="btn add" onClick={onCreate}>생성</div>
            <div className="btn remove" onClick={onRemove}>제거</div>
        </div>
    );
}

export default Buttons;
```

```javascript
// CounterList.js
import React from 'react';
import Counter from './Counter';
// 이 형태의 배열을 만들 것

const CounterList = ({counters, onIncrement, onDecrement, onSetColor}) => {
    const counterList = counters.map(
        (counter, i) => {
            return (
            <Counter key={i} index={i} {...counter}
            onIncrement={onIncrement} onDecrement={onDecrement}
            onSetColor={onSetColor}/>
        )}
    );
    return (
        <div className="CounterList">
            {counterList}
        </div>
    );
};
CounterList.defaultProps = {
    counters: []
}
export default CounterList;
```

```javascript
// Counter.js
import React from 'react';

const Counter = ({number, color, index, onIncrement, onDecrement, onSetColor}) => {
  return (
    <div
      className="Counter"
      onClick={() => onIncrement(index)}
      onContextMenu={(e) => {
      e.preventDefault();
      onDecrement(index);
    }}
      onDoubleClick={() => onSetColor(index)}
      style={{
      backgroundColor: color
    }}>
      {number}
    </div>
  );
};

Counter.defaultProps = {
  index: 0,
  number: 0,
  color: 'black',
  onIncrement: () => console.warn('onIncrement not defined'),
  onDecrement: () => console.warn('onDecrement not defined'),
  onSetColor: () => console.warn('onSetColor not defined')
};

export default Counter;
```

## 컨테이너 생성

```javascript
import CounterList from '../components/CounterList';
import * as actions from '../actions';
import {connect} from 'react-redux';
import getRandomColor from '../lib/getRandomColor';

const mapStateToProps = (state) => ({counters: state.counters});

// store 안의 state 값을 props로 연결

const mapDispatchToProps = (dispatch) => ({
    onIncrement : (index) => dispatch(actions.increment(index)),
    onDecremant : (index) => dispatch(actions.decrement(index)),
    onSetColor : (index) => {
        const color = getRandomColor();
        dispatch(actions.setColor({index, color}));
    }
})

const CounterListContainer = connect(mapStateToProps, mapDispatchToProps)(CounterList);

export default CounterListContainer;
```

버튼에 대한 컨테이너가 없기 때문에 앱에서 해당 내용 처리

```javascript
import React, { Component } from 'react';
import Buttons from '../components/Buttons';
import CounterListContainer from './CounterListContainer';
import getRandomColor from '../lib/getRandomColor';
import {connect} from 'react-redux';
import * as actions from '../actions';

class App extends Component {
    render() {
        const {onCreate, onRemove} = this.props;
        return (
            <div className="App">
                <Buttons onCreate={onCreate} onRemove={onRemove} />
                <CounterListContainer />
            </div>
        );
    }
}

const mapToDispatch = (dispatch) => ({
    onCreate: () => dispatch(actions.create(getRandomColor())),
    onRemove: () => dispatch(actions.remove())
});

export default connect(null, mapToDispatch)(App);
```

