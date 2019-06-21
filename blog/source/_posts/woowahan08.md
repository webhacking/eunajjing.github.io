---
title: 우아한테크러닝 Typescript & React 101 08
date: 2019-06-19 14:45:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

> 7회차의 경우 기존에 썼던 코드들을 다시 한 번 되새김질 하는 라이브 코딩으로 진행한 터라, 따로 필기하지 않음

# 비동기 처리 시 UI 제어

유저에게 무언가가 실행 중일 때 이를 알리기 위해 UI 단에 가시적으로 스피너를 많이 사용하는데, 해당 스피너 제어 로직에 대해 다뤄보았다.

## 스피너의 종류

일반적으로 해당 스피너는 두 개의 종류로 나뉜다.

- 전역 스피너
  - 어플리케이션에서 무슨 작업이 일어날 때마다 유저에게 보이는 스피너
- 지역 스피너
  - 어플리케이션 일부에 종속되어, 해당 컴포넌트 내에서 특정 작업이 일어날 때만 유저에게 보이는 스피너
  - 이 경우 자신이 종속된 컴포넌트가 무엇을 요청했는지 알아야 한다.
    - 유니크 키가 필요하다.

## 로직에 대해 고민해보자

뷰에 `button`이 하나 있고, 해당 `button`이 `click` 되었을 때 비동기 처리 요청이 실행된다고 가정한다.

1. `click` 이 되었을 때 비동기 요청 `action`이 `dispatch` 된다.
2. `redux-saga`는 해당 `action` 객체를 받아 로직을 처리한다.
3. `saga`가 `api`를 요청해 `call` 객체를 받게 된다.
4. `call`은 `api`에 요청했던 데이터를 가지고 있거나, 실패했을 것이다.
5. **스피너에게 있어 요청 성공과 실패는 같다**, 어차피 스피너가 사라져야 하기 때문
6. 성공 시 `saga`는 `put`을 통해 `reducer`에게 `dispatch`한다.

**`button`은 어떻게 `api` 요청 성공/실패 여부를 받는가**. 상태를 만드는 방법도 있겠지만, 비동기 요청이 많이 사용된다면 상태 값이 엄청 많아진다는 문제가 생긴다. 알림을 쌓듯 **상태를 배열로 넣는다**.

### 배열에는 무엇이 들어가는가

비동기 관련 데이터

- 요청의 상태(진행 중/완료)
- 발생 일시
- 키
  - 이전 것과 이후 것을 비교하기 위한 역할

## 코드를 만들어보자

### 타입의 정의, `store/types.ts`

#### `FinishStatus`

```typescript
export type FinishStatus = "success" | "error";
```

비동기의 상태는 `success`이거나, `error`

#### `IAsyncTaskStatus`

```typescript
export interface IAsyncTaskStatus {
  id: string; // 유니크 아이디를 만들어주는 uuid 라이브러리에서 생성해주는 id가 string이라서
  complete: boolean;
  // 완료 여부
  timestamp: number;
  // 언제 발생한 비동기인지
  
  // 위까지는 필수 요소
 	
  action: string;
  // 트리거한 액션 타입을 로그로 남기려고 만든 타입
  completeStatus?: FinishStatus;
  // 비동기 상태 여부를 알기 위해
}
```

```typescript
export interface IStoreState {
	// 위의 타입 중략
  asyncTasks: IAsyncTaskStatus[];
}
```

### 초기값 세팅 `reducers/index.ts`

```typescript
export const initializeState: IStoreState = {
  // 다른 값들 생략
  asyncTasks: []
};
```

### 비동기 관련 `action` 생성 `actions/index.ts`

```typescript
import { createAsyncPayload } from "./toolkit";

// 비동기 시작할 때 디스패치 할 액션
export const createAsyncTask = createAction(
  "@command/async-task/create",
  resolve => (id: string, action: string) => {
    return resolve({ id, action });
    // 상태 배열 안에 들어가는 타 데이터들은 사가 쪽에서 생성
  }
);

// 비동기 작업 종료시 디스패치 할 액션
export const completeAsyncTask = createAction(
  "@command/async-task/complete",
  resolve => (id: string, completeStatus: FinishStatus = "success") => {
    return resolve({ id, completeStatus });
  }
);

// 배열이 너무 커지는 것을 방지하기 위해, 일정 시간이 지난 비동기 작업은 삭제하는 로직 구현
// 해당 로직은 사가에서 구현을 해도 되고, interval()을 이용해도 무방하다.
export const cleanupAsyncTask = createAction(
  "@command/cleanup/async-task",
  resolve => () => resolve()
);

export const requestLogin = createAction(
  "@request/login",
  resolve => (username: string, password: string) =>
    resolve(createAsyncPayload({ username, password }))
  	// 위의 코드는 resolve({ username, password, asyncTrackingId: uuid() })와 같다.
);

export const requestShopList = createAction("@request/shop/list", resolve => {
  return () => resolve(createAsyncPayload());
});
```

#### `action` 생성 헬퍼 함수들을 따로 빼둔다. `toolkit.ts`

```typescript
import uuid from "uuid/v4";
// 유니크 id 생성할 라이브러리 가지고 오고

// 비동기 액션 생성 시 id를 넣어줌
export const createAsyncPayload = (payload = {}, prefix = "") => ({
  ...payload,
  asyncTaskId: `${prefix}${uuid()}`
});

// 비동기 액션의 id를 반환함
export const getAsyncId = action => action.payload.asyncTaskId;

// 해당 값이 넘어오는지를 boolean 값으로 확인
export const isAsyncAction = action => !!action.payload.asyncTaskId;
```

### `sagas/index.ts`

```typescript
import { all, fork, take, select, delay, put, call, takeLatest } from "redux-saga/effects";
import { isAsyncAction, getAsyncId } from "../actions/toolkit";
import { getType } from "typesafe-actions";
import * as Actions from "../actions";
import * as Api from "../apis/orders";
import { IStoreState, IAuthentication } from "../store";

function* watchRequestShopList() {
  while (true) {
    const action = yield take(getType(Actions.requestShopList));

    try {
      const resp = yield call(Api.fetchShops);
      // api에 넘어오는 데이터 자체에 id가 있다
      // 프로미스 객체를 통해 돌아오는 resp는 단일 컨텍스트를 지니고 있어서 처리가 심플하다
      
			yield put(Actions.completeAsyncTask(Actions.getAsyncId(action)));
			// 비동기 처리 객체의 id를 반환, 기본값이 성공이기 때문에 인자를 주지 않아도 됨
			
      yield put(Actions.responseShopList({ rows: resp.rows }));
      // 실제 비동기 처리
    } catch (e) {
      if (e instanceof Api.ApiError) {
      	yield put(Actions.addNotification("error", e.errorMessage));
      	// 알림 처리
        yield put(
          Actions.completeAsyncTask(Actions.getAsyncId(action), "error")
        );
				// 비동기 처리 객체의 id를 반환, 기본값이 성공이기 때문에 인자를 줘야 함
      } else {
        console.error(e);
      }
    }
  }
}

function* watchAsyncTask() {
  while (true) {
    const action = yield take("*");
    // 모든 액션을 가져와서

    // if (action.payload.asyncTaskId) {
    //   // 해당 액션이 비동기 테스크 아이디를 가지고 있으면
    			// 이것도 코드가 너무 길어지니까 헬퍼 함수 작성
    // }
    if (isAsyncAction(action)) {
      // yield put(
      //   Actions.createAsyncTask(action.payload.asyncTaskId, action.type)
      // );
      // action.payload.asyncTaskId를 빼내는 헬퍼 함수 작성
      yield put(Actions.createAsyncTask(getAsyncId(action), action.type));
      // 나머지 빈 데이터들은 리듀서 단에서 작성
    }
  }
}

function* fetchOrderTimeline() {
  const { results: { successTimeline, failureTimeline } }
  = yield call(Api.fetchOrderTimeline, moment().format("YYYYMMDD"));
  yield put(Actions.updateOrderTimeline(successTimeline, failureTimeline));
}

function* watchFetchOrderTimeline() {
  yield takeLatest(getType(Actions.showOrderTimelineChart), fetchOrderTimeline);
  // 여러 요청 중에서 마지막 들어온 요청의 응답만을 처리
  // 첫 인자로 들어오면 실행할 액션의 타입
  // 두 번째 인자로 실행할 제너레이터 함수를 넘긴다
}

function* cleanupAsyncTaskStats() {
  while (true) {
    yield delay(1000 * 60);
    // 1분이 지나면 비동기 요청 배열을 지운다
    yield put(Actions.cleanupAsyncTask());
  }
}

export default function*() {
  yield fork(watchRequestShopList);
  yield fork(watchAsyncTask);
  yield fork(fetchOrderTimeline);
  yield fork(watchFetchOrderTimeline);
  yield fork(cleanupAsyncTaskStats);
  // 콜러에게 제어권을 넘기며 백그라운드 단에서 실행할 함수 등록
}
```

### case 추가 `reducers/index.ts`

```typescript
// import 구문 생략
// 타입 정의 생략

export default (state: IStoreState = initializeState, action: ActionType<typeof Actions> ) => {
  switch (action.type) {
    case getType(Actions.createAsyncTask):
      return {
        ...state,
        asyncTasks: [
          ...state.asyncTasks,
          {
            // 새로 들어온 것 액션 관련 데이터 추가
            id: action.payload.id,
            action: action.payload.action,
            // id, action은 사가에서 넘어오고
            
            // 무조건 시작점에는 실패니까
            complete: false,
            timestamp: Date.now()
          }
        ]
      };
		case getType(Actions.completeAsyncTask):
      return {
        ...state,
        asyncTasks: state.asyncTasks.map(task =>
          task.id === action.payload.id
          // 비동기 상태가 변경된 id가 배열 안에서 발견되면                               
            ? {
                ...task,
                complete: true,
                completeStatus: action.payload.completeStatus
              }
            : { ...task }
        )
      };
		case getType(Actions.cleanupAsyncTask): {
      const currentTime = Date.now();
      return {
        ...state,
        asyncTasks: state.asyncTasks.filter(
          // 비동기 작업 데이터 배열에 필터를 돌려
          task => task.complete && currentTime - task.timestamp > 10000
          // 비동기 작업이 끝났고, 요청 시간이 현재 시간보다 1분이 지난 것들을 남김 (???)
        )
      };
    }
	// 그 외 case 문 생략
	}
}
```

### 지역적 스피너

#### `Container`

해당 컨테이너는 비동기의 시작과 끝을 알기 위한 상태와, 비동기 id를 필요로 한다.

##### `hook`으로 만들 경우

> ### `actions/toolkit.ts`
>
> 해당 헬퍼 함수 추가
>
> ```typescript
> export const isFinishAsyncAction = (asyncTasks: IAsyncTaskStatus[], id: string)
> // 비동기 작업 상황이 들어있는 배열과 비동기 작업 id를 인자로 받아
> => !!asyncTasks.find((t: IAsyncTaskStatus) => t.id === id && t.complete);
> // 배열의 id가 같으며 비동기 작업이 끝난 값이 있는지 여부(boolean)를 리턴
> ```

```typescript
import * as React from "react";
import { connect } from "react-redux";
import { IStoreState, IShop, IAsyncTaskStatus } from "../store";
import { ShopList as ShopListComponent } from "../components";

interface IProps {
  shopList: IShop[];
  asyncTasks: IAsyncTaskStatus[];
  requestShopList(): object;
}

const mapStateToProps = (state: IStoreState) => ({
  asyncTasks: state.asyncTasks,
  shopList: state.shopList
});

const ShopListContainer: React.FC<IProps> = props => {

	const [requestStatus, setRequestStats] = React.useState(false);
  const [requestShopListId, setRequestShopListId] = React.useState("");

	React.useEffect(() => {
		const action = props.requestShopList();
		// props로 받은 requestShopList()을 실행하며,
		// 해당 함수는 액션 생성 함수이며 생성 시 비동기 액션 id를 생성하는 toolkit 함수를 호출한다.
		setRequestStats(true);
    setRequestShopListId(getAsyncId(action));
   	// 액션을 인자로 보내 액션 id를 뽑아내고, 이를 hook에 저장
	}, []);
	// 두 번째 인자가 빈 배열이므로 이 useEffect는 컴포넌트 최초 마운팅에만 실행된다.
  
  React.useEffect(() => {
    if (isFinishAsyncAction(props.asyncTasks, requestShopListId)) {
      // 비동기 처리가 끝났다면
      setRequestStats(false);
    }
  }, [props.asyncTasks]);
  // 두 번째 인자에 값이 있으므로, 해당 배열이 변경될 때(업데이트될 때) 실행된다.

  return <Component data={props.shopList} loading={requestStatus} />;
  
}

export const ShopListContainer = connect(
  mapStateToProps,
  {requestShopList}
)(ShopList);
```

### 전역 스피너

#### `containers/Layout.tsx`

> #### `actions/toolkit.ts`
>
> ```typescript
> import { IAsyncTaskStatus } from "../store";
> 
> export const hasRunningAsyncAction = asyncTasks =>
>   asyncTasks.filter((t: IAsyncTaskStatus) => !t.complete).length > 0;
> // 비동기 작업 목록 중에서 미완료된 것이 있는지 boolean으로 리턴
> ```

```typescript
import * as React from "react";
import { connect } from "react-redux";
import { IStoreState } from "../store";
import { Layout } from "antd";
import { hasRunningAsyncAction } from "../actions";
import { Sidebar } from "../components";

interface IProps {
  location?: any;
  runningAsyncTask: boolean;
  children?: React.ReactNode;
  // 기타 타입들 생략
}

const mapStateToProps = (state: IStoreState) => ({
  runningAsyncTask: hasRunningAsyncAction(state.asyncTasks),
  // 기타 상태 생략
});

class LayoutContainer extends React.PureComponent<IProps> {
  render() {
    return (
      <Layout style={{ height: "100vh" }}>
      <Sidebar
          location={this.props.location}
          runningAsyncTask={this.props.runningAsyncTask}
        />
            {/* 기타 react node 생략*/}
      </Layout>
	)}
}
```

#### `components/Sidebar.tsx`

```typescript
import * as React from "react";
import { Layout, Spin } from "antd";
import { Maybe } from "./Maybe";

const { Header, Sider } = Layout;

interface IProps {
  runningAsyncTask: boolean;
  // 기타 props 생략
}

export const Sidebar: React.FC<IProps> = props => {
  return (
    <Sider width="250">
      <Header>
    		<Maybe test={props.runningAsyncTask}>
          <Spin style={{ float: "left", marginLeft: 90, marginTop: -8 }} />
        </Maybe>
      </Header>
		{*/ 기타 노드 생략/*}
    </Sider>
  )
}
```

> # 아직 해결하지 못한 것들
>
> ## `toolkit.ts`의 헬퍼 함수, `getLastFinishAction`
>
> ```typescript
> import last from "lodash/last";
> 
> export const getLastFinishAction = (asyncTasks: IAsyncTaskStatus[], action) => {
>   return last(
>     asyncTasks
>       .filter((t: IAsyncTaskStatus) => t.action === action)
>       .sort((a: IAsyncTaskStatus, b: IAsyncTaskStatus) =>
>         a.timestamp > b.timestamp ? 1 : a.timestamp < b.timestamp ? -1 : 0
>       )
>   );
> };
> ```
>
> 해당 헬퍼 함수가 어디서 실행되는지 모르겠고, `lodash/last` 패키지가 뭘하는 건지 모르겠다.
>
> 일단 인자로 들어온 `action`을 `asyncTasks`에서 찾은 다음, 정렬하는 것 같은데.... 정확히 뭘 하는지 모르겠고, 이 소스코드 설명을 소스를 직접 보면서 들은 게 아니라서... 필기로는 *비동기 작업 완료 후 해당 액션의 아이디로 로그(type) 빼는 헬퍼 함수*라고 되어있긴 한데 잘 모르겠다...
>
> ### `hook`으로 만든 컴포넌트를 일반 컴포넌트로 변경 시 발생하는 에러
>
> 지역 스피너를 돌렸던 `shopListContanier`를 처음에 `hook`으로 작성했다가 일반 퓨어 컴포넌트로 만드는 작업을 진행했는데, 예제 자체에서도 에러가 난다. 좀 살펴보면 고칠 수 있을 것 같긴 한데.... 일단 문제가 되는 소스 붙여넣기 후 나중에 봐야지 ~~이러고 절대 안볼 가능성 농후하지만~~
>
> ```typescript
> import * as React from "react";
> import { connect } from "react-redux";
> import { IStoreState, IShop, IAsyncTaskStatus } from "../store";
> import { requestShopList, getAsyncId, isFinishAsyncAction } from "../actions";
> import { ShopList as ShopListComponent } from "../components";
> 
> interface IProps {
>   shopList: IShop[];
>   asyncTasks: IAsyncTaskStatus[];
>   requestShopList(): object;
> }
> 
> interface IState {
>   requestStatus: boolean;
>   requestShopListId: string;
> }
> 
> const mapStateToProps = (state: IStoreState) => ({
>   asyncTasks: state.asyncTasks,
>   shopList: state.shopList
> });
> 
> class ShopList extends React.Component<IProps, IState> {
>   state = {
>     requestStatus: false,
>     requestShopListId: ""
>   };
> 
>   componentDidMount() {
>     const action = this.props.requestShopList();
> 
>     this.setState({
>       requestStatus: true,
>       requestShopListId: getAsyncId(action)
>     });
>   }
> 
>   componentDidUpdate() {
>     if (
>       isFinishAsyncAction(this.props.asyncTasks, this.state.requestShopListId)
>     ) {
>       this.setState({
>         requestStatus: false
>       });
>     }
>   }
> 
>   render() {
>     return (
>       <ShopListComponent
>         data={this.props.shopList}
>         loading={this.state.requestStatus}
>       />
>     );
>   }
> }
> /*
> const ShopList: React.FC<IProps> = props => {
>   const [requestStatus, setRequestStats] = React.useState(false);
>   const [requestShopListId, setRequestShopListId] = React.useState("");
> 
>   React.useEffect(() => {
>     const action = props.requestShopList();
>     setRequestStats(true);
>     setRequestShopListId(getAsyncId(action));
>   }, []);
> 
>   React.useEffect(() => {
>     if (isFinishAsyncAction(props.asyncTasks, requestShopListId)) {
>       setRequestStats(false);
>     }
>   }, [props.asyncTasks]);
> 
>   return <ShopListComponent data={props.shopList} loading={requestStatus} />;
> };
> */
> export const ShopListContainer = connect(
>   mapStateToProps,
>   {
>     requestShopList
>   }
> )(ShopList);
> ```