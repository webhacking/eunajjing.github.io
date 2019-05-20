---
title: 우아한테크러닝 Typescript & React 101 02
date: 2019-05-20 16:28:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

# redux

- 리액트와 별개의 상태 관리 라이브러리

- 용어

  - `status` : 앱을 만들 때 쓰는 데이터

  - `store` : 상태가 들어가있음

- 리덕스는 아무리 큰 앱일지라도 단일 스토어를 권장

- `createStore()`는 스토어를 만들어주는 함수

  ```javascript
  const store = createStore(reducer);
  ```

- 상태가 바뀔 때마다 스토어를 아예 새로 만드는 정책을 사용해서 관리

> - 스냅샷
> - 깃과 비슷하다

## `createStore()`의 필수 인자, `reducer`

- 상태를 관리할 수 있게 돕는 역할

- 리덕스는 직접 상태를 바꿀 수 없고, 리덕스를 이용해 판별한다

- 리덕스는 두 개의 객체를 인자로 받는다

  - 상태
    - 이걸 이용해서 상태 처리 후 리턴 값으로 바뀐 상태 객체를 리턴한다
  - 액션 객체
    - 반드시 `type`이라는 키를 가지며 value는 문자열이다

- 리듀서 자체가 새로운 객체를 직접 만들지 못하기 때문에 만들어줘야 한다

  (즉 새로운 상태를 담고 있는 객체를 직접 만들지 못함)

  - 만드는 방법에는 두 가지가 있다

    - ES6의 전개 연산자 `...`

    - `Object.assign({}, objectName)`

      - 2 depth 이상 복사 불가

      > # Object.assign(object, object)
      >
      > ```javascript
      > const target = { a: 1, b: 2 };
      > const source = { b: 4, c: 5 };
      > 
      > const returnedTarget = Object.assign(target, source);
      > // 첫 인자로 바꿀 객체,
      > // 두 번째 인자로 바꾸는데 사용할 객체
      > // 해당 함수는 바뀐 객체를 리턴함(target)
      > 
      > console.log(target);
      > // expected output: Object { a: 1, b: 4, c: 5 }
      > 
      > console.log(returnedTarget);
      > // expected output: Object { a: 1, b: 4, c: 5 }
      > ```

## 스토어에 있는 함수들

### 상태가 바뀌었는지 알고 싶다면? `subscribe()`

- 스토어가 제공하는 함수 중 하나로, 상태가 바뀔 때마다 호출됨
- 인자로 상태가 바뀔 때 실행할 함수를 넣어준다
- 이를 `스토어에 함수를 등록한다`고 말한다

### 상태를 바꿀 때 `dispatch()`라는 함수 호출

- `dispatch()`에는 액션 객체가 인자로 들어간다

## 액션 생성 함수의 등장

- 매번 `dispatch()`를 할 때마다 액션을 새로 만들어줘야 하는 번거로움

- 차라리 액션을 만들어주는 생성 함수를 만드는 건 어떨까

  ```javascript
  (...)
  
  function actionCreate = (a, b) => {
    return {
      type : "블라블라",
      payload : {
        a : a,
        b: b
      }
    }
  }
  
  dispatch(actcionCreate(a, b));
  ```

## 바닐라와 함께하는 redux

```javascript
import {createStore} from 'redux';
// 리덕스는 디폴트 패키지가 없다

const x = {
  type: "change load status"
}

const InitializeState = {
  load : false
};

const reducer = (state = InitializeState, action) => {
// es6 문법으로 디폴트 값을 지정해준다
// 그래서 아래의 코드를 쓸 필요가 없음

  // if (state === undefined) {
  //   // 리덕스가 직접 초기값을 설정해주지 못해 직접 해줘야 함
  //   state = {
  //     load : false
  //   };
  // }

  switch(action.type) {
    // 타입을 보고 어떻게 상태를 바꿀 것인지 확인

    case "change load status":
    return {
      ...state,
      load: !state.load
    };
    default :
    return Object.assign({},state);
  }

  return state;
}

const store = createStore(reducer);

store.subscribe(()=> {
  console.log("change");
  console.log(store.getState());
})

store.dispatch({
  type: "change load status"
});

// 디스패치가 되면 subscribe에 등록했던 함수가 호출됨
```

### check in, check out 예제

```javascript
import {createStore} from 'redux';

const CHECKIN = "@action/checkin";
const CHECKOUT = "@action/checkout";

const InitializeState = {
  checkInStatus : false,
  checkOutStatus : false,
  checkInTimestamp: 0,
  checkOutTimestamp: 0,
  visitorName : ""
};

const reducer = (state = InitializeState, action) => {

  switch(action.type) {
    case CHECKIN:
    return {
      ...state,
      checkInStatus: true,
      checkInTimestamp: Date.now(),
      checkOutTimestamp: 0,
      visitorName : action.payload.visitorName
      // payload라는 상위 값을 만들어줄 것
    };
    case CHECKOUT:
    return {
      ...state,
      checkInStatus: true,
      checkOutStatus : true,
      checkOutTimestamp: Date.now()
      // 입력하지 않은 다른 값들은 그대로 남는다
    };
    default :
    return {...state};
  }

  return state;
}

const store = createStore(reducer);

store.subscribe(()=> {
  console.log(store.getState());
})

store.dispatch({
  type: CHECKIN,
  payload : {
    visitorName: "테스트"
  }
});
store.dispatch({
  type: CHECKOUT
});
```

## redux 이용의 어려움

- 모델링이 어렵다
  - depth가 길어지면 핸들링이 어렵다
- 리덕스에서는 리듀서가 순수 함수여야 한다고 말한다
  - 사이드 이팩트가 일어나는 함수는 쓰지 말 것을 권고
  - 기본적으로 동기 플로우
  - 네트워크 콜, 비동기 작업은 어떻게 할 것인가?
    - 미들웨어 아키텍쳐 사용
      - api call을 미들웨어 쪽으로 뺀다
      - 이럴 경우 플로우가 복잡해짐
        - ui + 리듀서 + 비동기 처리 미들웨어 플로우에 관한 고민
        - 어떤 로직을 어디다가 넣어야 하는지에 대한 고민이 생긴다

## React와 Redux의 결합

### `index.tsx`

```javascript
import * as React from "react";
import { render } from "react-dom";

import { Provider } from "react-redux";
// 리덕스를 보다 잘 쓸 수 있도록 래핑한 패키지
// 스토어의 상태가 변경될 때마다 해당 상태를 받아갈 컴포넌트들을 프로바이더의 자식으로 두어야 함

import { createStore } from "redux";
import { StoreState } from "./types";
// export interface StoreState {
//  success: number;
//  failure: number;
// }

import reducers from "./reducers";
import App from "./App";

const store: StoreState = createStore(reducers);
const rootElement: HTMLElement = document.getElementById("root");

render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement
);
```

#### `redux.ts(index.ts)`

```javascript
import { StoreState } from "../types";
import { FETCH_SUCCESS, FETCH_FAILURE } from "../actions/action-type";
// 찍힐 로그 설정
// export const FETCH_SUCCESS = "@fetch/success";
// export const FETCH_FAILURE = "@fetch/failure";

import * as Actions from "../actions";
// 타입 지정 및 액션 생성 함수 생성 역할

// import { FETCH_SUCCESS, FETCH_FAILURE } from "./action-type";
// export interface CommandAction {
//   type: typeof FETCH_SUCCESS | typeof FETCH_FAILURE;
//   // OR 연산자, 둘 중 하나
//   payload: null;
// }

// export const fetchSuccess = (): CommandAction => ({
//   type: FETCH_SUCCESS,
//   payload: null
// });
// // 객체를 리턴하는 경우 ({}); 사용

// export const fetchFailure = (): CommandAction => ({
//   type: FETCH_FAILURE,
//   payload: null
// });

const initializeState: StoreState = {
  success: 0,
  failure: 0
};

function mainReducer(state: StoreState = initializeState, action: Actions.CommandAction){
  switch (action.type) {
    case FETCH_SUCCESS:
      return {
        ...state,
        success: state.success + Math.floor(Math.random() * (100 - 1) + 1)
      };
    case FETCH_FAILURE:
      return {
        ...state,
        failure: state.failure + Math.floor(Math.random() * (2 - 0))
      };
    default:
      return Object.assign({}, state);
  }
}

export default mainReducer;
```

#### `<App/>`은, `<App/>`의 children들은 어떤 상태를 받나?

- 어떤 상태를 받을 것인지 선택할 수 있다!

### `App.tsx`

```javascript
import * as React from "react";
import { Dispatch } from "redux";
import { connect } from "react-redux";
// 스토어의 변경 사항을 구독하겠다고 감싸줄 역할
import { MonitorCard, PlayButton } from "./components";
import { fetchSuccess, fetchFailure } from "./actions";
import { StoreState } from "./types";
import { Typography } from "antd";

import "antd/dist/antd.css";
import "./sass/main.scss";

interface Application {
  timerId: number;
  onStart(): void;
  onStop(): void;
}

interface AppProps {
  success: number;
  failure: number;
  fetchSuccess(): void;
  fetchFailure(): void;
}

const mapStateToProps = (state: StoreState) => ({
  // 부모 프로바이더가 스토어가 가진 state를 모두 파라미터로 던져주고
  // 자식 app에서는 어떤 상태 값을 받을 것인지 객체 형태로 기입
  // 여기서는 그냥 다 받겠다고 해둠
  ...state
});

const mapDispatchToProps = (dispatch: Dispatch) => ({
  // 스토어가 제공하는 디스패치 메서드가 이 컴포넌트에는 없는데
  // 아래에서 실행되는 커넥트 함수가 디스패치 메서드를 주입해주는 것
  // 자바스크립트의 클로저
  // 커넥트 함수가 이 mapDispatchToProps 함수를 인자로 받으면
  // mapDispatchToProps에 디스패치 함수를 주입
  
  // fetchSuccess와 fetchFailure는 객체이며
  // 키의 값이 함수
  // 디스패치 함수가 액션 생성 함수를 실행시켜 액션을 만들어 인자로 가져감
  fetchSuccess: () => {
    dispatch(fetchSuccess());
  },
  fetchFailure: () => {
    dispatch(fetchFailure());
  }
});

class MonitorApp extends React.PureComponent<AppProps> implements Application {
  timerId: number = 0;

  onStart = () => {
    this.timerId = setInterval(() => {
      this.props.fetchSuccess();
      this.props.fetchFailure();
    }, 200);
  };

  onStop = () => {
    clearInterval(this.timerId);
    this.timerId = 0;
  };

  render() {
    return (
      <div>
        <header>
          <Typography.Title>React & TS Boilerplate</Typography.Title>
          <Typography>Order Monitor</Typography>
        </header>
        <main>
          <MonitorCard
            success={this.props.success}
            // 부모가 넘겨주는 props에서 success가 오기 때문에
            // this.props.success가 되는 것
            failure={this.props.failure}
          />
          <PlayButton
            monitoring={false}
            onPlay={this.onStart}
            onPause={this.onStop}
          />
        </main>
      </div>
    );
  }
}

const App = connect(
  mapStateToProps,
  mapDispatchToProps
)(MonitorApp);
// 커넥트는 리턴으로 함수를 준다
// 리턴 결과로 나온 함수에게 컴포넌트를 인자로 주는 것
// 즉, 어떤 상태를 해당 컴포넌트가 받을 것임을 묶는다

// 커넥트 함수의 인자는 통상 2개의 함수다.
// mapStateToProps라는 함수는 스토어의 상태 중 이 컴포넌트가 뭘 가져올 것인지 알려주고 주입
// mapDispatchToProps라는 함수는 데이터를 받기만 하면 없어도 되는데 액션을 디스패치할 경우
// 디스패치할 함수를 주입시킨다

export default App;
```

## 보일러 플레이트에 redux 붙이기

```javascript
import * as React from "react";
import { OrderStatusContiner, MonitorControllerContainer } from "./containers";
import { Typography } from "antd";

import "antd/dist/antd.css";
import "./sass/main.scss";

export default class App extends React.PureComponent {
  render() {
    return (
      <div>
        <header>
          <Typography.Title>React & TS Boilerplate</Typography.Title>
        </header>
        <main>
          <OrderStatusContiner />
      		// Counter, MonitorCard 컴포넌트
          <MonitorControllerContainer />
      		// playButton 컴포넌트
        </main>
      </div>
    );
  }
}
```

### `acitons - index.ts`

```javascript
import { createAction } from "typesafe-actions";

export const startMonitoring = createAction(
  "@command/monitoring/start",
  resolve => {
    return () => resolve();
  }
);

export const stopMonitoring = createAction(
  "@command/monitoring/stop",
  resolve => {
    return () => resolve();
  }
);

export const fetchSuccess = createAction("@fetch/success", resolve => {
  return () => resolve();
});

export const fetchFailure = createAction("@fetch/failure", resolve => {
  return () => resolve();
});
```

### `containers` - `OrderStatus.tsx`

```javascript
...
render() {
    return (
      <MonitorCard>
        <Counter title="Success" count={this.props.success} />
        <Counter title="Failure" count={this.props.failure} color="red" />
        <Counter title="Error Rate" count={this.state.errorRate} unit="%" />
        {/* count로 재사용 */}
      </MonitorCard>
      // 타입 스크립트의 딜레마
      // 모니터카드의 칠드런의 카운트에 특정 값만 넣고 싶은데 어떻게 타입을 기입하는가
      // 방법이 없다
    );
  }
```

**`count`라는 속성이 어떻게 재사용되는지에 주목**

```javascript
import * as React from "react";
import { FormattedNumber } from "./FormattedNumber";

const DEFAULT_UNIT = "";
const DEFAULT_COLOR = "#000";

interface CounterProps {
  title: string;
  count: number | string;
  color?: string;
  // 해당 속성은 타이틀 속성이 Failure이었을 때만 들어온다
  unit?: string;
  // 해당 속성은 타이틀 속성이 Error Rate이었을 때만 들어온다
}

export const Counter: React.FC<CounterProps> = props => {
  const unit = props.unit || DEFAULT_UNIT;
  const color = props.color || DEFAULT_COLOR;

  return (
    <div className="item">
      <p>{props.title}</p>
      <p style={{ color }}>
        {/* 컬러 객체가 스타일 속성의 값으로 들어온다 */}
        <FormattedNumber value={props.count} />
        <span className="unit">{unit}</span>
      </p>
    </div>
  );
};
```

# 라이프 사이클의 순서

1. `componentDidMount()`

   : ui 가시화된 직후

2. `componentDidUpdate(prevProps)`

   - 상태가 바뀌어서 렌더링이 다시 되고 호출된 것
   - `setState` 같은 거 하면 안됨 (무한루프)

   - `prevProps`는 업데이트 전의 상태

     - 만약 현재 것과 비교해서 쓸 일이 있다면 사용됨

       ```javascript
       componentDidUpdate(prevProps) {
           if (prevProps.monitoring !== this.props.monitoring) {
             // 모니터링 여부가 바뀌면
             if (this.props.monitoring) {
               // 모니터링 시작
               this.timerId = setInterval(() => {
                 this.props.fetchSuccess();
                 this.props.fetchFailure();
               }, 200);
             } else {
               // 상태 하나가 바뀌어도 계속 다시 호출되기 때문에
               // 이를 감안한 코드를 작성해야함
               // 즉 다른 상태가 바뀌어서 실행되었을 때에 대한 조건문이 필요
               clearInterval(this.timerId);
               this.timerId = null;
             }
           }
       
           if (
             prevProps.success !== this.props.success ||
             prevProps.failure !== this.props.failure
           ) {
             // 수신 받은 데이터가 바뀌면
             this.setState({
               // 원래 이렇게 하지 않기를 권고하나 계속 상태 값이 바뀌는 예제로...
               errorRate:
                 this.props.failure > 0
                   ? Number((this.props.failure / this.props.success) * 100).toFixed(2)
                   : "0"
             });
           }
         }
       
       ```

3. `componentWillUnmount()`

   : 이 컴포넌트 사라지기 직전 호출, 초기화한다던가

