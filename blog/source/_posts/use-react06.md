---
title: 리액트를 다루는 기술06
date: 2019-03-22 17:13:18
tags:
categories:
- 개발공부
- React
---

# 미들웨어

```javascript
const loggerMiddleware = store => next => action => {
    console.log('현재 상태', store.getState());
    console.log('액션', action);

    const result = next(action);
    // 다음 차례(다른 미들웨어, 리듀서)로 넘긴다

    console.log('액션 처리 후', store.getState());
    console.log('그래서 리턴 값이', result);
    // store.dispatch(ACTION_TYPE)
    console.log('\n');

    return result;
    // store.dispatch(ACTION_TYPE)
}

export default loggerMiddleware;
```

스토어 만드는 곳에서

```javascript
import { createStore, applyMiddleware } from 'redux';
import modules from './modules';
import loggerMiddleware from './lib/loggerMiddleware';

const store = createStore(modules, applyMiddleware(loggerMiddleware));
/* 미들웨어가 여러 개일 때는 콤마로 주며 순서는 여기에서 전달한 파라미터 순.
applyMiddleware(a, b, c);
*/

export default store;
```

# `redux-logger` 사용

`yarn add redux-logger`

