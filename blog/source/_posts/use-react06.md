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

# 외부 미들웨어의 사용

**redux-logger** 

```javascript
import {createLogger} from 'redux-logger';

const logger = createLogger();
const store = createStore(modules, applyMiddleware(logger));
```

**비동기 작업 처리 미들웨어**

- `redux-thunk`
- `redux-promise-middleware`
- `redux-pender`

## `redux-thunk`

객체가 아닌 함수도 디스패치할 수 있게 하는 미들웨어

리턴되는 함수는 액션 생성 함수가 아니라, `thunk` 생성 함수라 한다.

### `thunk`

특정 작업을 나중에 하기 위해 함수로 감싼 것

### `thunk` 생성 함수

- `dispatch`와 `getState`를 파라미터로 가지는 새로운 함수를 만들어서 반환
- 네트워크 요청, 다른 종류의 액션들을 여러 번 디스패치도 가능

### 사용

```javascript
const ACTCION_NAME = 'ACTCION_NAME';

function actionCreateFuc() {
    return {
      type : ACTION_NAME  
    };
}

function actionCreateAsyne() {
    return dispatch => {
        // 디스패치를 파라미터로 가지는 펑션을 리턴하는 것
        setTimeout(()=> {
            dispatch(actionCreateFuc());
        }, 1000);
    }
}
```

```javascript
function ignore() {
    retrun (dispatch, getState) => {
        cosnt {store} = getState();
        // 리턴하는 함수의 파라미터로 디스패치와 겟스테이터스를 주기 때문에, 스토어 상태에 접근 가능
        
        if (store % 2 === 0) {
            // 조건문 실행해서 조건문 맞으면 디스패치 미실행
            retrun;
        }
        
        dispatch(actionCreateFuc());
        
    };
}
```

-*일반 액션 객체로는 특정 액션을 디스패치한 후 몇 초 뒤에 실제로 반영시키거나 현재 상태에 따라 아예 무시하게 만들 수 없다*-

### 예제 반영

```javascript
// module.js
import { handleActions, createAction } from 'redux-actions';

const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

export const increment = createAction(INCREMENT);
export const decrement = createAction(DECREMENT);

export const incrementAsync =() => dispatch => {
    setTimeout(
        () => {dispatch(increment())},
        1000
    );
}
export const decrementAsync =() => dispatch => {
    setTimeout(
        () => {dispatch(decrement())},
        1000
    );
}

export default handleActions({
    [INCREMENT]: (state, action) => state + 1,
    [DECREMENT]: (state, action) => state - 1
}, 0);
```

```javascript
// app.js
class App extends Component {
    render() {
        const { CounterActions, number } = this.props;
        
        return (
            <div>
                <h1>{number}</h1>
                <button onClick={CounterActions.incrementAsync}>+</button>
                <button onClick={CounterActions.decrementAsync}>-</button>
            </div>
        );
    }
}
```

## `axios`

`yarn add axios`

```javascript
import axios from 'axios';


class App extends Component {
    componentDidMount() {
        axios.get('https://jsonplaceholder.typycode.com/posts/1')
        .then(response => console.log(response));
    }
    render() {
        
    }
}
```

## `redux-thunk`와 `axios`의 사용

```javascript
// post.js

import {handleActions, createAction} from 'redux-actions';
import axios from 'axios';

function getPostAPI(postId) {
    return axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`);
}

const GET_POST_PENDING = 'GET_POST_PENDING';
const GET_POST_SUCCESS = 'GET_POST_SUCCESS';
const GET_POST_FAILURE = 'GET_POST_FAILURE';

const getPostPending = createAction(GET_POST_PENDING);
const getPostSuccess = createAction(GET_POST_SUCCESS);
const getPostFailure = createAction(GET_POST_FAILURE);

export const getPost = (postId) => dispatch => {
    dispatch(getPostPending());
    return getPostAPI(postId).then((response) => {
        dispatch(getPostSuccess(response))
        // 요청이 성공했다면 서버 응답 내용을 payload로 설정해 성공 액션을 디스패치
        // then에 전달하는 함수에서 response에 접근할 수 있게 한다.
        return response;
    }).catch(error => {
        dispatch(getPostFailure(error));
        // 에러 처리를 던지되 한 번 더 catch 되도록 설정
        throw(error);
    })
}

const initialState = {
    pending : false,
    error : false,
    data : {
        title : '',
        body: ''
    }
}

export default handleActions({
    [GET_POST_PENDING] : (state, action) => {
        return {
            ...state,
            pending: true,
            error : false
        };
    },
    [GET_POST_SUCCESS] : (state, action) => {
        const {title, body} = action.payload.data;
        return {
            ...state,
            pending: false,
            error : false,
            data : {
                title,
                body
            }
        };
    },
    [GET_POST_FAILURE] : (state, action) => {
        return {
            ...state,
            pending: false,
            error: true
        }
    }
}, initialState);
```

```javascript
// app.js
import React, { Component } from 'react';
import { bindActionCreators } from 'redux';
import { connect } from 'react-redux';
import * as counterActions from './modules/counter';
import * as postActions from './modules/post';


class App extends Component {

    loadData = () => {
        const {PostActions, number} = this.props;
        PostActions.getPost(number).then(
            (response) => {
                console.log(response);
            }
        ).catch(
            (error) => {
                console.log(error);
            }
        );
    }

    componentDidMount() {
        this.loadData();
    }

    componentDidUpdate(prevProps, prevState) {
        if (this.props.number !== prevProps.number) {
            this.loadData();
        }
    }

    render() {
        const { CounterActions, number, post, error, loading } = this.props;
        
        return (
            <div>
                <h1>{number}</h1>
                { loading ? (<h2>로딩 중...</h2>)
                :   (error ? (<h2>오류 발생!</h2>)
                            : (<div><h2>{post.title}</h2>
                                    <p>{post.body}</p>
                                </div>)
                    )
                }
                <button onClick={CounterActions.increment}>+</button>
                <button onClick={CounterActions.decrement}>-</button>
            </div>
        );
    }
}

export default connect(
    (state) => ({
        number: state.counter,
        post: state.post.data,
        loading: state.post.pending,
        error: state.post.error
    }),
    (dispatch) => ({
        CounterActions: bindActionCreators(counterActions, dispatch),
        PostActions : bindActionCreators(postActions, dispatch)
    })
)(App);
```

> # async/await
>
> 만약 이런 코드가 있다고 가정한다면,
>
> ```javascript
> PostActions.getPost(number).then(
>          (response) => {
>              console.log(response);
>          }
>      ).catch(
>          (error) => {
>              console.log(error);
>          }
>      );
> ```
>
> ES7의 문법으로, 위의 코드를 이렇게 작성 가능하다.
>
> ```javascript
> try {
>             const response = await PostActions.getPost(number);
>             console.log(response);
> } catch(e) {
>             console.log(e);
> }
> ```
>
> 바벨 설정에 `Async to generator transform`  플러그인을 적용한다면 사용 가능한 문법.
>
> await를 쓸 함수 앞에 `async` 라는 키워드를 붙여주고, 기다려야 할 프로미스 앞에 `await` 키워드를 붙여주어야 한다.
>
> `await`를 사용했다면 반드시 `try - catch` 구문을 사용해 오류를 처리해야 한다.

## `redux-promise-middleware`

`yarn add redux-promise-middleware`

Promise 객체를 payload로 전달하면 요청을 시작, 성공, 실패할 때 액션의 뒷부분에 `_PENDING`, `_FULFILLED`, `_REJECTED`를 붙여서 반환한다.

각 앤션 타입을 선언할 필요가 없으며, 뒤에 붙는 접미사는 커스터마이징도 가능하다. 커스터마이징 방법은 아래와 같다.

```javascript
import { createStore, applyMiddleware } from 'redux';
import modules from './modules';
import {createLogger} from 'redux-logger';
import {createPromise} from 'redux-promise-middleware';

const logger = createLogger();

const pm = createPromise({
    promiseTypeSuffixes: ['PENDING', 'SUCCESS', 'FAILURE']
});

const store = createStore(modules, applyMiddleware(logger, pm));

export default store;
```

> `redux-promise-middleware` 미들웨어의 버전업으로 인해 책 내 기술된 사용 방법과 다르다. 자세한 사항은 해당 미들웨어 공홈에서 알 수 있었다.

```javascript
import {handleActions} from 'redux-actions';
import axios from 'axios';

function getPostAPI(postId) {
    return axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`);
}

const GET_POST = 'GET_POST';

export const getPost = (postId) => ({
    type: GET_POST,
    payload: getPostAPI(postId)
});
```

## `redux-pender`

액션 객체 안의 `payload` 가 `Promise`  형태라면 시작하기 전, 완료 또는 실패를 했을 때 뒤에 `PENDING`, `SUCCESS`, `FAILURE` 접미사를 붙인다. 요청을 관리하는 리듀서가 포함되어 있으며 요청 관련 액션들을 처리하는 액션 핸들러 함수들을 자동으로 만들어주는 도구들도 들어 있다. 요청 중인 액션을 취소할 수도 있다.

```javascript
// post.js
import { createStore, applyMiddleware } from 'redux';
import modules from './modules';
import {createLogger} from 'redux-logger';
import penderMiddleware from 'redux-pender';

const logger = createLogger();
const store = createStore(modules, applyMiddleware(logger, penderMiddleware()));

export default store;
```

```javascript
// index.js
import { combineReducers } from 'redux';
import counter from './counter';
import post from './post';
import {penderReducer} from 'redux-pender';

export default combineReducers({
    counter,
    post,
    pender: penderReducer
});
```

해당 리듀서는 요청 상태를 관리한다. 이 리듀서의 상태 구조는 다음과 같다.

```javascript
{
    pending : {},
    success: {},
    failure: {}
}
```

프로미스 기반 액션을 디스패치하면 상태는 이렇게 변경된다.

```javascript
{
    pending : {
        'ACTION_NAME': true
    },
    success: {
        'ACTION_NAME': false
    },
    failure: {
        'ACTION_NAME': false
    }
}
```

요청이 끝나고, 해당 요청이 성공이면

```javascript
{
    pending : {
        'ACTION_NAME': false
    },
    success: {
        'ACTION_NAME': true
    },
    failure: {
        'ACTION_NAME': false
    }
}
```

반대로, 해당 요청이 실패이면

```javascript
{
    pending : {
        'ACTION_NAME': false
    },
    success: {
        'ACTION_NAME': false
    },
    failure: {
        'ACTION_NAME': true
    }
}
```

모듈의 코드는 다음과 같이 변경된다.

```javascript
// post.js
import {handleActions, createAction} from 'redux-actions';
import axios from 'axios';
import {pender} from 'redux-pender';
 
function getPostAPI(postId) {
    return axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`);
}

const GET_POST = 'GET_POST';
export const getPost = createAction(GET_POST, getPostAPI);

const initialState = {
    data : {
        title : '',
        body: ''
    }
    // 연결했는지, 에러가 있는지 여부는 따로 관리를 할 필요가 없어진다.
}

export default handleActions({
    ...pender({
        type: GET_POST,
        onSuccess: (state, action) => {
            // onPending과 onFailure가 존재
            const {title, body} = action.payload.data;
            return {
                data : {
                    title,
                    body
                }
            }
        }
    })
}, initialState);
```

만약 여러 개를 관리한다면, `...pender` 를 여러 번 사용하거나 `applyPenders`를 사용하면 된다.

### `applyPenders`

당연한 이야기겠지만 `...pender`와 같이 쓰니 에러가 난다.

```javascript
// post.js 모듈
import {handleActions, createAction} from 'redux-actions';
import axios from 'axios';
import {applyPenders} from 'redux-pender';
 
function getPostAPI(postId) {
    return axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`);
}

const GET_POST = 'GET_POST';
export const getPost = createAction(GET_POST, getPostAPI);
const initialState = {
    data : {
        title : '',
        body: ''
    }
}

const reducer = handleActions({
    // 다른 일반 액션 관리
}, initialState);

export default applyPenders(reducer, [
    // 첫 번째 파라미터는 일반 리듀서, 두 번째 파라미터는 pender 관련 객체를 배열로
    {
        type: GET_POST,
        onSuccess: (state, action) => {
            const {title, body} = action.payload.data;
            return {
                data : {
                    title,
                    body
                }
            }
        }
    },
    // 다른 pender 액션들을 위와 같은 객체 형태로 쓴다.
]);
```

```javascript
// App.js의 커넥트에서도

export default connect(
    (state) => ({
        number: state.counter,
        post: state.post.data,
        loading: state.pender.pending['GET_POST'],
        error: state.pender.failure['GET_POST']
    }),
    (dispatch) => ({
        CounterActions: bindActionCreators(counterActions, dispatch),
        PostActions: bindActionCreators(postActions, dispatch)
    })
)(App);
```

### `@@redux-pender`

Promise 기반 액션을 시작하면 액션 두 개가 디스패치된다.

- `ACTION_NAME_RESULT`(ex : `GET_POST_PENDING`)

- `@@redux-pender/RESULT`(ex : `@@redux-pender/PENDING`)

  : 해당 액션의 payload 값에는 액션 이름이 들어가고, 이에 따라 pender 리듀서의 상태가 변화된다.

  ![](images/redux-pender.png)

### `onCancel`

요청을 취소했을 때 특정 작업을 하고 싶다면, `...pender`를 사용하는 부분에서 `onCancel` 함수를 추가하면 된다. **단, 이 함수는 웹 요청을 취소하는 게 아니라 무시하는 것일 뿐**

```javascript
// post.js

export default handleActions({
    ...pender({
        type: GET_POST,
        onSuccess: (state, action) => {
            const {title, body} = action.payload.data;
            return {
                data : {
                    title,
                    body
                }
            }
        },
        onCancel: (state, action) => {
            return {
                data : {
                    title: '요청 취소',
                    body: '요청 취소'
                }
            }
        }
    })
}, initialState);
```

> 참고
>
> ```javascript
> // app.js
> class App extends Component {
> 
>     cancelRequest = null
>     handleCancel = () => {
>         if(this.cancelRequest) {
>             this.cancelRequest();
>             this.cancelRequest = null;
>         }
>     }
> 
>     loadData = async () => {
>         const { PostActions, number } = this.props;
> 
>         try {
>             const p = PostActions.getPost(number);
>             this.cancelRequest = p.cancel;
>             const response = await p;
>             console.log(response);
>           } catch (e) {
>             console.log(e);
>           }
>     }
> 
>     componentDidMount() {
>         this.loadData();
>         window.addEventListener('keyup', (e) => {
>             if(e.key === 'Escape') {
>                 this.handleCancel();
>             }
>         })
>     }
> 
> (...)
>  
> }
> ```
>
> 

