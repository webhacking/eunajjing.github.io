---
title: 리액트를 다루는 기술05
date: 2019-02-27 16:06:18
tags:
categories:
- 개발공부
- React
---

# `Immutable`

```javascript
const {Map} = Immutable;
const data = Map({
  a: 1,
  b: 2,
  c: Map({
    d: 3,
    e: 4,
    f: 5
  })
});
```

여러 층으로 구성된 객체를 만들 때 매번 `Map({})`을 쓰는 게 귀찮으므로 이를 해결해줄

## `fromJS` 사용

```javascript
const {Map, fromJS} = Immutable;
const data = fromJS({
  a: 1,
  b: 2,
  c: {
    d: 3,
    e: 4,
    f: 5
  }
});
```

### 사용법

- `data.toJS()` : 자바스크립트 객체로 변환

- `data.get('a')` : 특정 키의 값을 불러올 때

- `data.getIn(['c', 'd'])` : 내부의 키 값을 가져오려고 할 때

  - c 안의 d의 값을 부르는 것

- `data.set('a', 4)` : 새로운 값을 넣은 map 생성

  - 기존 map이 수정되는 것이 아니다 

  - `const newData = data.set('a', 4)`

    ```javascript
    const {Map, fromJS} = Immutable;
    const newData = fromJS({
      a: 4,
      b: 2,
      c: {
        d: 3,
        e: 4,
        f: 5
      }
    });
    ```

- `data.setIn(['c', 'd'], 10)` : 내부의 키에 값을 넣는 map을 생성하려고 할 때

  - 마찬가지로 기존 객체가 변경되지는 않는다.

- `data.updateIn(['c', 'd'], 불리언값 => !불리언값)` : 해당 값을 바꾸려 할 때

- `data.merge({a: 10, b: 10})` : 값 여러 개를 동시에 바꾸려고 할 때

- `data.mergeIn(['c'], {d: 10, e: 10})` : 내부의 값 여러 개를 동시에 바꾸려고 할 때

  - 체이닝 방식으로도 기술 가능하다

    ```javascript
    const newData = data.setIn(['c', 'd'], 10)
    					.setIn(['c', 'e'], 10);
    ```

## `List` 사용

배열 대신 사용하는 것

`map`, `filter`, `sort`, `push`, `pop`을 내장하고 있다.

마찬가지로 List 자체를 변경하는 게 아니라, 새로운 List를 반환한다.

```javascript
const {List} = Immutable;
const list = List([0, 1, 2, 3, 4,]);
```

객체들의 list를 만들어야 할 때는

```javascript
const {List, Map, fromJS} = Immutable;
const list1 = List([
    Map({value: 1}),
    Map({value: 2})
]);

// or

const list2 = fromJS([
   {value: 1},
   {value: 2}
]);
```

메서드들도 똑같이 쓸 수 있다. 

- `list.toJS()`

- `list1.get(0)` // 이 경우 반환되는 것은 `{value:1}` 객체

- `list.getIn([0, 'value'])` // 이 경우 0번 째 방의 value라는 키의 값, 즉 1

- `list.set(0, Map({value: 10}))` // 이 경우 0번째 방의 객체를 바꾸겠다

- `list.setIn([0, 'value'], 10)` // 이 경우 0번째 방의 객체의 value 키의 값을 바꾸겠다

  - `list.update(0, item => item.set('value', item.get('value')*5))`

    // 기존 값을 참고해서 업데이트 해야할 때

    // 0번 방의 value 필드의 값을 기존 value 값에 * 5를 한 것으로 바꾸겠다

    - 첫 번째 파라미터 : 선택할 인덱스 값
    - 두 번째 파라미터 : 선택한 원소를 업데이트하는 메서드

  - `list.setIn([0, 'value'], list.ginIn([0, 'value']*5))`와 같다

- `list.push(Map({value:3}))`

  - **마찬가지로 기존 것에 push 하는 게 아니라, 새로운 것을 만들어 push한다는 것에 유의**

- `list.unshift(Map({value:0}))` // 맨 앞에 넣고 싶을 때

- `list.delete(1)` // 1번 방에 있는 객체 삭제

  - `list.pop()`은 맨 끝에 있는 것

- `list.size` // **length가 아니다**

- `list.isEmpth()`도 가능

# bindActionCreators

`connect` 시에 사용하는 `mapDispatchToProps` 메서드에 사용할 수 있는 외부 모듈로 각각의 액션에 `dispatch` 설정을 해줄 필요가 없다.

```javascript
export default connect(
    (state) => ({
        value: state.input.get('value')
    }),
    (dispatch) => ({
        inputActions: bindActionCreators(inputActions, dispatch),
        /*
        inputActions: {
        	setInput: (value) => dispatch(inputActions.setInput(value))
        	// 액션생성함수들을 자동으로 연결시킴
    	}
        */
        todosActions: bindActionCreators(todosActions, dispatch)
        // 첫 번째 파라미터는 액션 생성 함수가 들어있는 객체 모듈, 두 번째 파라미터는 dispatch
    })
)(TodoInputContainer);
```

# Ducks

액션 타입, 액션 생성 함수, 리듀서 모두 한 파일에서 모듈화하여 관리하는 파일 구조

## 규칙

- `export default`를 이용하여 리듀서, 액션 생성 메서드를 내보내야 한다.
- 액션 타입의 명명규칙은 `npm-module-or-app/reducer/ACTION_TYPE` 형식이다.
  - 여러 프로젝트로 나누거나 라이브러리를 만든 게 아니라면 `npm-module-or-app/reducer/`은 생략이 가능하다.
- 외부 리듀서에서 액션 타입이 필요할 때는 액션 타입 또한 내보내도 된다.

# `redux-actions`을 통한 관리

`yarn add redux-action`으로 의존 설정을 해준 뒤

`import {createAction, handleActions} from 'redux-actions'`으로 불러 실행한다.

## createAction

액션 생성 메서드를 단축한다.

```javascript
// 액션 생성 메서드
export const increment = createAction(types.INCREMENT);
export const setColor = createAction(types.SET_COLOR);
// setColor는 파라미터를 계속 객체로 받는다면,
// 가독성을 위해 해당 부분을 명시해도 좋다.
// export const setColor = createAction(types.SET_COLOR, ({index, color}) => ({index, color}))

// 생성할 때
increment(3);
setColor({index:5, color:'#fff'})
/*
{
    type: 'INCREAMENT',
   	payload : 3
}

{
    type: 'SET_COLOR',
    payload : {
        index : 5,
        color: '#fff'
    }
}
*/
```

## handleActions

```javascript
const reducer = handleActions({
    INCREMENT: (state, action) => ({
        counter: state.counter + action.payload
    }),
    INCREMENT: (state, action) => ({
        counter: state.counter - action.payload
    })
}, {counter: 0});
// 첫 번째 파라미터는 액션에 따라 실행할 함수를 가진 객체를 넣는다
// 두 번째 파라미터는 기본 값을 넣는다
```