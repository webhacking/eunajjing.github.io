---
title: 리액트를 다루는 기술05 (실습)
date: 2019-03-21 16:23:18
tags:
categories:
- 개발공부
- React
---

# Ducks 모듈 구현

## `input.js`

```javascript
import {Map} from 'immutable';
import {handleActions, createAction} from 'redux-actions';

const SET_INPUT = 'input/SET_INPUT';
// 액션타입의 이름을 js이름/액션타입으로 정해준다.

export const setInput = createAction(SET_INPUT);
// 액션 생성함수

const initialState = Map({
    value: ''
});
// 초기 값

export default handleActions({
    [SET_INPUT]: (state, action) => {
        return state.set('value', action.payload)
    }
}, initialState);
// 리듀서 정의
```

## `todos.js`

```javascript
import {Map, List} from 'immutable';
import {handleActions, createAction} from 'redux-actions'

const INSERT = 'todos/INSERT';
const TOGGLE = 'todos/TOGGLE';
const REMOVE = 'todos/REMOVE';

export const insert = createAction(INSERT);
export const toggle = createAction(TOGGLE);
export const remove = createAction(REMOVE);

const initaialState = List([
    Map({
        id: 0,
        text: '리액트 공부하기',
        done: true
    }),
    Map({
        id: 1,
        text: '컴포넌트 스타일링 해보기',
        done: false
    })
]);

export default handleActions({
    [INSERT]: (state, action) => {
        const {id, text, done} = action.payload;
        // 이 액션이 어떤 데이터를 처리하는지 알기 위해 기입
        return state.push(Map({
            id,
            text,
            done
        }));
        // 그냥 state.push(Map(action.payload)) 해도 된다.
    },
    [TOGGLE]: (state, action) => {
        const { payload: id} = action;
        // const id = action.payload와 같다. payload의 값을 쉽게 볼 수 있게 비구조화 할당 진행
        const index = state.findIndex(todo => todo.get('id')===id);
        return state.updateIn([index, 'done'], done => !done);
        // state.setIn([index, 'done'], !state.getIn([0, index]));
    },
    [REMOVE]: (state, action) => {
        const {payload: id} = action;
        const index = state.findIndex(todo => todo.get('id')===id);
        return state.delete(index);
    }
}, initaialState);
```

## `index.js`

```javascript
import input from './input';
import todos from './todos';
import {combineReducers} from 'redux';

export default combineReducers({
    input,
    todos
});
```

# `index.js`에서는

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './styles/main.scss';
import App from './components/App';
import * as serviceWorker from './serviceWorker';
import modules from './modules';
import { createStore } from 'redux';
import { Provider } from 'react-redux';

const store = createStore(modules, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());
// 스토어를 모듈로 만들어 넣는다는 게 핵심

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>, document.getElementById('root'));

serviceWorker.unregister();

```

# Container의 생성

```javascript
// TodoInputContainer.js
import React, { Component } from 'react';
import TodoInput from '../components/TodoInput';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';

import * as inputActions from '../modules/input';
import * as todosActions from '../modules/todos';

class TodoInputContainer extends Component {
    id = 1
    getId = () => ++this.id;
    handleChange = (e) => {
        console.log("handleChange");
        const { value } = e.target;
        const { inputActions } = this.props;
        inputActions.setInput(value);
    }
    handleInsert = () => {
        console.log("handleInsert");
        const { inputActions, todosActions, value } = this.props;
        const todo = {
            id: this.getId(),
            text: value,
            done: false
        };
        todosActions.insert(todo);
        inputActions.setInput('');
    }
    render() {
        const { value } = this.props;
        /* 해당 value는
        import * as inputActions from '../modules/input';
        import * as todosActions from '../modules/todos';
        중 inputActions에서 온다
        */
        const { handleChange, handleInsert } = this;
        return (
            <TodoInput onChange={handleChange} onInsert={handleInsert} value={value} />
        );
    }
}

export default connect(
    (state) => ({
        value: state.input.get('value')
    }),
    (dispatch) => ({
        inputActions: bindActionCreators(inputActions, dispatch),
        todosActions: bindActionCreators(todosActions, dispatch)
    })
)(TodoInputContainer);
```

```javascript
//TodoListContainer.js
import React, { Component } from 'react';
import TodoList from '../components/TodoList';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import * as todosActions from '../modules/todos';

class TodoListContainer extends Component {
    handleToggle = (id) => {
        const { todosActions } = this.props;
        todosActions.toggle(id);
    }
    handleRemove = (id) => {
        const { todosActions } = this.props;
        todosActions.remove(id);
    }
    render() {
        const { todos } = this.props;
        // 해당 todos는 import * as todosActions from '../modules/todos';요기에서 온다
        const { handleRemove, handleToggle } = this;
        return (
            <TodoList todos={todos} onToggle={handleToggle} onRemove={handleRemove} />
        );
    }
}

export default connect(
    (state) => ({
        todos: state.todos
    }),
    (dispatch) => ({
        todosActions: bindActionCreators(todosActions, dispatch)
    })
)(TodoListContainer);
```

