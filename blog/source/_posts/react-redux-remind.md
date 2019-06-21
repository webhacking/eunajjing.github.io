---
title: React Redux 복습
date: 2019-06-10 22:04:18
tags:
categories:
- 개발공부
- React
---

# Redux

`createStore()`를 통해 스토어를 만들고, 인자로 리듀서 함수를 넣어 만든다. 사용할 때는 `react-redux`에서 제공하는 `Provider`를 통해 속성으로 넣어준다.

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { createStore } from "redux";
import rootReducer from "./store/modules";
import { Provider } from "react-redux";

const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

## `combineReducers()`

여러 개의 리듀서 함수들을 하나로 뭉쳐주는 역할을 한다. 리덕스는 단일 스토어 원칙을 따르기 때문에, 리듀서 함수는 여러 개일 수 있어도 스토어는 하나다. 리듀서 함수들을 객체로 만들어 넣어준다.

```javascript
import { combineReducers } from "redux";
import counter from "./counter";
import waiting from "./waiting";

export default combineReducers({ counter, waiting });
```

## `reducer`

`ducks` 패턴으로 만든다.

- 액션 타입 정의
- 액션 생성 함수 작성
- 리듀서 함수

### `action` 타입 정의

```javascript
const CHANGE_INPUT = "waiting/CHANGE_INPUT";
```

나중에 여러 개의 리듀서와 합쳐서 쓸 것이므로, 어디서 만들어진 액션인지 알기 위해 일반적으로 `리듀서함수명/액션타입명`으로 만든다.

### `action` 생성 함수 작성

일단 액션은 이렇게 생겼다.

```javascript
const actionTest = {
	type : 'actionType',
	payload : {
		blahblah : blahblah
	}
}
```

물론 원한다면 `payload` 를 쓰지 않아도 되긴 하는데, 그렇게 하면 액션마다 뭐가 있는지을 알아야 하는 번거로움이 있기 때문에 `payload`를 쓰고 그 안에다가 data를 넣는다.

이 작업이 너무 귀찮기 때문에 보통 `redux-actions` 패키지에서 제공하는 `createAction` 함수를 쓴다.

```javascript
export const changeInput = createAction(CHANGE_INPUT);
```

이렇게 해도 상관은 없지만, 이 코드를 작성하지 않은 이가 본다면 이 액션 생성 함수는 어떤 인자로 필요로 하는지 알 수 없기 때문에 인자까지 넣어서 작성해주는 게 일반적이다.

```javascript
export const changeInput = createAction(CHANGE_INPUT, text => text);
```

### reducer 함수

들어오는 `action` 의 타입을 받아 어떤 로직을 실행할지 결정하기 때문에 `switch` 문을 쓴다. 인자로는 상태와 액션을 받는다.

> `switch` 의 스코프는 첫 시작부터 default까지

근데 `switch` 문 작성도 귀찮기 때문에, 번거로움을 해소하기 위하여 `redux-actions` 패키지에서 제공하는 `handleActions`를 쓴다.

#### `handleActions`

- 첫 파라미터로는 객체가 들어오며, `switch`와 거의 형태가 유사하다.
- 두 번째 파라미터로 초기값이 들어온다.

```javascript
export default handleActions(
  {
    [CHANGE_INPUT]: (state, action) => ({
      ...state,
      input: action.payload
    }),
    [CREATE]: (state, action) => ({
      ...state,
      list: state.list.concat({
        ...action.payload,
        entered: false
      })
    }),
    [ENTER]: (state, action) => ({
      ...state,
      list: state.list.map(item => {
        if (item.id === action.payload) {
          return {
            ...item,
            entered: !item.entered
          };
        }
      })
    }),
    [LEAVE]: (state, action) => ({
      ...state,
      list: state.list.filter(item => item.id !== action.payload)
    })
  },
  initialState
);
```

> 리듀서 함수는 순수 함수로만 구성되어야 한다.
>
> 만약 리듀서 함수 내부에서 무언가를 조작해야하는 상황이라면, 미들웨어를 사용하거나 액션 생성 함수 작성 시에 활용한다.
>
> 아래는 `id`라는 인자가 1씩 증가해야할 때 작성한 액션 생성함수
>
> ```javascript
> import { createAction } from "redux-actions";
> 
> let id = 0;
> export const ActionTest = createAction('ActionType', pram => ({pram, id : id++}));
> ```
>
> > ES6 문법에 따라 `pram` 으로 들어오는 key와 value를 함께 쓴 것

## 컨테이너에서 사용할 때

- `mapStateToProps` 를 정의
  - 해당 컨테이너에서 사용할 state를 가져옴
- `mapDispatchToProps`를 정의
  - 해당 컨테이너에서 사용할 dispatch를 가져옴

### `mapDispatchToProps` 정의

```javascript
const mapDispatchToProps = dispatch => ({
  increment: () => dispatch(increment()),
  decrement: () => dispatch(decrement())
});
```

```javascript
import { bindActionCreators } from "redux";

const mapDispatchToProps = dispatch =>
  bindActionCreators({ increment, decrement });
```

두 개의 코드는 같다. 즉, `bindActionCreators` 액션생성함수를 가지고 있는 `dispatch` 함수를 생성한다. 그런데 리덕스는 `bindActionCreators`를 쓰지 않아도 자동화 되어서,

```javascript
const mapDispatchToProps = { increment, decrement };
```

이렇게 액션 생성함수를 객체로 만들어서 넣어줘도 같다.