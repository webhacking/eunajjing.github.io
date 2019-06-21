---
title: React middleware
date: 2019-06-10 21:42:18
tags:
categories:
- 개발공부
- React
---

# 미들웨어의 사용

`Redux`의 `store`에도 접근할 수 있도록 `store`를 만들 적 설정해준다. 

```javascript
const store = createStore(modules, applyMiddleware(logger, ReduxThunk));

export default store;
```

## `applyMiddleware`

사용할 미들웨어들을 인자로 받는다. 인자로 받은 미들웨어들은 순차적으로 실행되게 된다. 이렇게 만든 `store`를 리덕스 스토어로 사용한다.

## `redux-logger`

```javascript
import { createLogger } from "redux-logger";

const logger = createLogger();
```

어떤 디스패치가 행해져 스토어의 값이 변경되었는지 로그를 찍어주는 패키지. `createLogger()`를 통해 만든 미들웨어를 `applyMiddleware()`의 인자로 넘긴다.

## `ReduxThunk`

```javascript
import ReduxThunk from "redux-thunk";
```

만약 인자로 넘어온 `action` 이 함수라면 자동으로 디스패치를 가로채는 패키지. 비동기 처리 작업을 위해 사용한다.

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

> # 왜 이런 작업을 수행하나?
>
> 실행되는 시점을 제어하기 위해. 즉, 호출 즉시 실행이 아니라 원하는 시점에 실행되길 바라기 때문

## 만약 직접 미들웨어를 만든다면

```javascript
const middleware = store => next => action => {
  console.log(action);
  console.log(store.getState());
  next(action);
  console.log(store.getState());
};

export default middleware;
```

미들웨어는 스토어의 값을 받아올 수 있으며, `next()` 를 이용해 다음 미들웨어로 액션을 넘길 수 있다.

> # 노드와 다른 점
>
> 노드의 경우 `next()` 구문을 만나면 하위의 구문은 무시된다. 반면 리액트의 미들웨어는 **계속 실행된다**

[이전에 첫 공부를 하며 했던 포스팅](https://eunajjing.github.io/2019/03/22/use-react06/)