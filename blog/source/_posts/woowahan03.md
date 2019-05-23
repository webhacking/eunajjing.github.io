---
title: 우아한테크러닝 Typescript & React 101 03
date: 2019-05-23 18:44:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

# redux-saga

사실 당일에 배운 내용이 더 있긴 한데, 포스트 하나로 쓰자니 스스로도 지치고 주제가 많이 쪼개져있는 터라 포스트 세 개로 분산시켜놓았다.

- [프로미스, 어싱크/어웨이](https://eunajjing.github.io/2019/05/22/promise-async-await/)
- [제너레이터](https://eunajjing.github.io/2019/05/22/generator/)

이 포스트는 두 개의 포스트와 이어져있다.

## 미들웨어

미들웨어는 약간 이런 구조로 되어있다. 미들웨어 내에서 사용되는 인자들은 클로저로 넣어준다.

```javascript
const middleware = (store) => {
  return (next) => {
    return (action) => {
      if (action.type === 'fetch user list') {
        // 미들웨어가 액션을 판단해서 실행된다
        (new Promise((res, rej) => {
          res([1, 2, 3])
        })).then(resp => {
          store.dispatch({
            type: '',
            payload : resp
          })
        })
      }
    }
  }
}
```

### 미들웨어의 역할

- 액션의 모니터링
- 스토어 참조
- 상태를 받을 수 있으며, 디스패치가 가능하다.

### 비동기 처리 미들웨어의 등장 이유

리덕스의 리듀서는 기본적으로 순수 함수여야만 한다. 즉, **부수 효과 작업이 불가하다**. 그런데 비동기 작업은 이 부수 효과 작업에 속하므로, 해당 작업을 처리할 미들웨어가 필요하게 되었다.

### 순서

- 미들웨어의 실행
  - 비동기 작업 진행
  - 상태를 새로 만들어(액션 객체) 디스패치
- 리듀서가 객체를 받음
  - 이 경우, 리듀서 입장에서는 해당 상태가 어떻게 온 것인지 상관하지 않아도 됨
  - 동기 데이터를 가지고 온 것과 같다

### `applyMiddleware`

리덕스 패키지의 함수 중 하나로, 미들웨어를 인자로 넣어준다.

```javascript
const sagaMiddleware = createSagaMiddleware();
// 사가 미들웨어 생성

const store: StoreState = createStore(reducer, applyMiddleware(sagaMiddleware));
```

## redux-saga

### `index.tsx`

```javascript
import createSagaMiddleware from "redux-saga";
import rootSaga from "./sagas";
// (그 외 임포트 구문 생략)

const sagaMiddleware = createSagaMiddleware();
const store: StoreState = createStore(reducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(rootSaga);
// rootSaga는 제너레이터 함수이다.
// 사가 실행
```

### `/sagas/index.ts`

#### 일단, 소스 타이핑 전에 `redux-saga/effects` 패키지에 관하여

##### `fork`

제너레이터 펑션을 인자로 받으며, 백그라운드로 인자로 넣은 펑션을 계속해서 실행하겠다는 뜻이다. 단, **호출하는 게 아니므로, 실행되진 않는다**. 그냥 백그라운드로 계속 실행할 의향이 있음을 등록한다.

```javascript
yield fork(monitoringWorkflow);
// 이 예제에서는 yield 키워드를 이용해 fork를 콜러에게 넘긴다.
// 콜러 측에서 run();을 했을 때 비로소 인자로 받은 제너레이터 펑션이 실행된다.
```

##### `put`

리듀서에게 액션을 디스패치한다.

```javascript
put(Actions.actionCreateFunc)
```

##### `all`

일반적으로 제너레이터 펑션은 `yield` 키워드를 만나면 그 함수를 잠시 중단 시킨다. 그러나 제너레이터 펑션을 콜러 측에서 여러번 호출해 동시에 사용할 경우, `yield` 키워드와 관계 없이 제너레이터 펑션이 **동시에 실행**되어야 하므로 사용한다.

만약 `all`을 사용하지 않는다면 해당 제너레이터 펑션은 `yield` 키워드를 만날 때마다 멈추므로 콜러 측에서 원활한 사용이 어렵다.

```javascript
yield all([
        put(Actions.fetchSuccess),
        put(Actions.fetchFailure)
      ]);
      // all 구문 안을 실행 중에도 펑션이 멈추지 않고 계속 진행하며 내려간다
```

##### `take`

특정 문자열이 들어오기를 기다린다.

```javascript
yield take('~');
```

위의 코드에서 인자로 들어온 문자열과 같은 값을 `take`이 받았을 경우, `yield`가 실행되어 콜러 측으로 `take('~')`가 전달된다.

##### `delay`

자바의 쓰레드 슬립과 비슷한 기능으로, 실행 중이던 함수를 잠깐 멈춘다. **함수를 동기적으로 사용할 수 있다**.

```javascript
delay(200)
```

##### `race`

제일 먼저 도달에 성공한 함수만을 실행 시킨다.

```javascript
yield race({
        waitting: delay(200),
        stoped: take(getType(Actions.stopMonitoring))
      });
```

위의 코드에서 액션 타입이 `stopMonitoring`으로 바뀌지 않는 한 계속 `waitting`이 이긴다.

##### `call`, `apply`

비동기를 사용할 때 쓰며, `async/await`의 `await`와 같은 역할을 수행한다.

#### `typesafe-actions`

- 리액트, 리덕스 타입스크립트와 함께 쓰이는 패키지
- [(영문)사용 설명서](https://github.com/piotrwitek/typesafe-actions)

##### `getType`

액션의 타입 값을 뽑아내는 함수

```javascript
import { getType } from "typesafe-actions";
import * as Actions from "../actions";

getType(Actions.startMonitoring);
```

##### `createAction`

액션을 생성할 때, 문자열이나 함수를 받을 수 있는 함수. 쓰는 방법은 너무나 다양하다.

자세한 방법은 [공식 문서 참조](https://github.com/piotrwitek/typesafe-actions#createaction)

```javascript
import { createAction } from 'typesafe-actions';

const increment = createAction('INCREMENT');
// 문자열 하나만 입력한다면, 해당 액션은 타입의 값이 해당 문자열이다.
// { type: 'INCREMENT' };

const add = createAction('ADD', action => {
  return (amount: number) => action(amount);
});
dispatch(add(10));
// { type: 'ADD', payload: number }

const getTodo = createAction('GET_TODO', action => {
  return (id: string, meta: string) => action(id, meta);
});
dispatch(getTodo('some_id', 'some_meta'));
// { type: 'GET_TODO', payload: string, meta: string }
```

> # 예제의, `actions/index.ts`
>
> ```javascript
> import { createAction } from "typesafe-actions";
> 
> export const startMonitoring = createAction(
>   "@command/monitoring/start",
>   resolve => {
>     return () => resolve();
>   }
> );
> // 위는 분명한 함수이며,
> // 인자값으로 문자열이나 함수를 주면 상태의 속성이 되어 들어간다.
> 
> export const stopMonitoring = createAction(
>   "@command/monitoring/stop",
>   resolve => {
>     return () => resolve();
>   }
> );
> 
> export const fetchSuccess = createAction("@fetch/success", resolve => {
>   return () => resolve();
> });
> 
> export const fetchFailure = createAction("@fetch/failure", resolve => {
>   return () => resolve();
> });
> ```

> ## 왜 이런 패턴을 쓰나요?
>
> 사실 나는 왜 이렇게 쓰는지 잘 모르겠어서 따로 질문을 드렸는데, 액션을 생성할 때마다 타입을 제외한 상태 속성을 계속 만들어주기 번거롭기 때문이라고 한다.
>
> > 사실 처음 질문한 의도는 `resolve => {...}` 이 구문이었는데 질문을 내가 잘못드린 것 같다... 근데 정리하다보니까 이해가 됐다.

#### 그래서 진짜 `index.ts`

```javascript
import { fork, all, take, race, delay, put } from "redux-saga/effects";
import { getType } from "typesafe-actions";
import * as Actions from "../actions";

function* monitoringWorkflow() {
  while (true) {
    // 이 반복문은 영원히 실행된다
    // 만약 한 번 실행되었다가 정지되었다면,
    // 이 함수는 계속 startMonitoring 타입 값을 기다릴 것이다
    yield take(getType(Actions.startMonitoring));
    // 콜러에게 take를 준다,
    // 액션 타입이 추출되어 문자열로 받는데
    // startMonitoring이라는 문자열을 기다린다

    // 문자열 들어오면 일드가 멈춘다
    // 즉, startMonitoring가 들어왔다는 것이 약속된다
    // 모니터링 시작

    let polling = true;

    while (polling) {
      yield all([
        put({ type: getType(Actions.fetchSuccess) }),
        // 디스패치한다
        put({ type: getType(Actions.fetchFailure) })
      ]);

      const { stoped } = yield race({
        // 액션 타입이 모니터링 정지가 될 때까지 waitting이 실행된다.
        waitting: delay(200),
        stoped: take(getType(Actions.stopMonitoring))
      });

      if (stoped) {
        polling = false;
      }
    }
  }
}

export default function*() {
  // 실제로 익스포트는 이 함수만 된다.
  // 이 함수는 index.tsx(앱 첫 진입점)에서 사가의 run()이 실행한다.
  yield fork(monitoringWorkflow);
}
```

> 아직까지 비동기로 처리하는 로직을 붙이지 않아서, 그게 덧붙여지면 해당 예제가 더 완벽히 이해가 될 것 같다!