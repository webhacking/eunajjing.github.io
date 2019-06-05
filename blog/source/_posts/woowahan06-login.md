---
title: 우아한테크러닝 Typescript & React 101 06 login
date: 2019-06-05 14:42:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

# 로그인과 로그아웃

> ## 비동기의 액션
>
> 액션은 크게 두 개, 혹은 세 개가 있어야 한다.
>
> - 액션의 요청
> - 액션의 응답
>   - 액션 응답이 성공
>   - 액션 응답이 실패

> 일단 전제로 로그인 관련 액션과 타입이 정의되어 있고, 해당 로직을 처리하는 리듀서가 있음
>
> 페이지 컴포넌트에서는 로그인 시 발급되는 토큰이 있는지 여부에 따라 해당 페이지의 렌더 여부가 결정됨

## saga

```javascript
import { all, fork, take, select, delay, put, call } from "redux-saga/effects";
import { getType } from "typesafe-actions";
import * as Actions from "../actions";
import * as Api from "../apis/orders";
import { IStoreState } from "../store";

function* authenticationWorkflow() {
  while (true) {
    try {
      const { payload: { username, password } } = yield take(getType(Actions.requestLogin));

      const response = yield call(Api.requestLogin, username, password);
      // 일단 콜이 api 함수 호출하면서 유저네임이랑 패스워드를 넘김
      // 프로미스 객체가 api 객체에게 넘어오면
      // 콜이 플레인 객체 만들고
      // 사가가 까보고 제대로 잘 들어왔으면 프로미스가 resolve면 response

      yield put(Actions.successLogin(response));
    } catch (e) {
      if (e instanceof Api.ApiError) {
        yield put(Actions.addNotification("error", e.errorMessage));
      } else {
        console.error(e);
      }
    }

    yield take(getType(Actions.requestLogout));
    // 해당 코드의 문제점은 캐치문에 잡혀도 영원히 로그아웃을 기다려야 함
    yield put(Actions.successLogout());
  }
}

export default function*() {
  yield fork(authenticationWorkflow);
}
```

## 문제 해결

### `createStore` 의 인자

- 리듀서
- 함수(미들웨어)
- 객체(스토어에 주입할 초기값)

**아예 스토어 생성 때 초기값 세팅을 하는 게 낫다.** 만약 컨테이너 단에서 초기값이 설정되면, 화면 깜빡임 현상이 발생할 가능성이 있다. 게다가 아래의 코드처럼 작성하면 세션에 이미 로그인 정보가 저장되어 있다면, 자동으로 로그인이 진행되어 넘어간다.

```javascript
import * as React from "react";
import { render } from "react-dom";
import { Provider } from "react-redux";
import { createStore, applyMiddleware } from "redux";
import { composeWithDevTools } from "redux-devtools-extension";
import { IStoreState } from "./store";
import reducer, { initializeState } from "./reducers";
import createSagaMiddleware from "redux-saga";
import rootSaga from "./sagas";
import App from "./router";

const authenticationData = sessionStorage.getItem("authentication");

const sagaMiddleware = createSagaMiddleware();
const store: IStoreState = createStore(
  reducer,
  authenticationData
    ? {
        ...initializeState,
        authentication: { ...JSON.parse(authenticationData) }
      }
    : initializeState,
  composeWithDevTools(applyMiddleware(sagaMiddleware))
);

const rootElement: HTMLElement = document.getElementById("root");
sagaMiddleware.run(rootSaga);

render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement
);
```

### saga

```javascript
import { all, fork, take, select, delay, put, call, race } from "redux-saga/effects";
import { getType } from "typesafe-actions";
import * as Actions from "../actions";
import * as Api from "../apis/orders";
import { IStoreState } from "../store";

function* authenticationWorkflow() {
  while (true) {
    let { authentication } = yield select();
    let waitLogin = !authentication;
    // 로그인이 되어 있다면 해당 값은 false
    // 로그아웃 상태라면 해당 값은 true

    while (waitLogin) {
      // 로그아웃 상태에서 로그인을 기다리는 로직
      // 위에서는 단순히 true 값으로 고정해서 루프를 태웠지만, 해당 코드는 주입한 초기값을 기반하여 진행
      try {
        const { payload: { username, password } } = yield take(getType(Actions.requestLogin));
        const { result } = yield call(Api.requestLogin, username, password);
        
        waitLogin = !waitLogin;
        // 로그인에 필요한 토큰을 사가의 call을 이용해 받아왔으니,
        // 로그인 루프를 벗어나기 위해 해당 값을 교체

        sessionStorage.setItem(
          "authentication",
          JSON.stringify({
            ...result
          })
        );
        // 세션에 로그인 토큰을 넣어준다

        yield put(Actions.successLogin({ ...result })); // 리듀서에게 디스패치
      } catch (e) {
        if (e instanceof Api.ApiError) {
          yield put(Actions.addNotification("error", e.errorMessage));
        } else {
          console.error(e);
        }
      }
    }
		
    // 로그아웃 로직
    yield take(getType(Actions.requestLogout));
    sessionStorage.removeItem("authentication");
    yield put(Actions.successLogout());
  }
}

export default function*() {
  yield fork(authenticationWorkflow);
}
```

