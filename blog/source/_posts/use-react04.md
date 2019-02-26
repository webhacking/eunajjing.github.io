---
title: 리액트를 다루는 기술04
date: 2019-02-26 15:09:18
tags:
categories:
- 개발공부
- React
---

# 리덕스

- `state`를 컴포넌트 밖에서 관리하기 위해 사용

## 용어

- `스토어`:  `state` 저장하고 있는 객체
- `액션` : 상태에 변화를 일으켜야 할 때 스토어에 전달하는 객체
- `리듀서` : 액션을 참고해서 스토어의 state를 어떻게 바꿀 것인지 결정해서 바꿈
- `디스패치` : 액션을 전달하는 행위

## 규칙

- 스토어는 단 한 개이다
- 리듀서는 여러 개를 만들 수 있다
- 모든 변화는 리듀서 함수로 일어나야 한다.
  - 리듀서 함수에서 외부 네트워크, 데이터베이스에 직접 접근해선 안된다.
  - 리듀서 함수 내부에서는 `new Date()`와 `Math.random()`을 사용하면 안된다.

## 액션

```javascript
{
    type: "INCREMENT"
}
```

- `type` 

  - 어떤 작업을 하는 액션인지 정의
  - 대문자와 _로 구성한다.
  - 필수 값이다.
  - 값은 고정이다.

- 예시

  ```javascript
  {
      type: "INCREMENT",
      text: '리액트 배우기'
  }
  ```

  ```javascript
  {
      type: "INCREMENT",
      todos: {
          id: 1,
          text: '리액트 배우기',
          done: false
      }
  }
  ```

### 액션 생성함수

```javascript
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const increment = (diff) => {
    type: INCREMENT
    diff: diff
}
const decrement = (diff) => {
    type: DECREMENT
    diff: diff
}
```

## 리듀서

- 파라미터
  - 현재 상태
  - 액션 객체
- initialState 값을 설정해준다

```javascript
// 액션 함수를 위에 쓴 뒤에
const initialState = {
    number: 0
};
```

- 리듀서 함수 시작

```javascript
function counter(state = initialState, action) {
    switch(action.type){
        case INCREMENT:
        	retrun {number: state.number + action.diff};
        case DECREMENT:
        	retrun {number: state.number - action.diff};
        default:
        	retrun state;
    }
}
```

- 만약 액션 객체 안에 많은 속성들이 있다면

```javascript
const initialState = {
    number: 0,
    foo: 'bar',
    baz: 'qux'
};

function counter(state = initialState, action) {
    switch(action.type){
        case INCREMENT:
        	retrun Object.assign({}, state, {
                number: state.number + action.diff
            });
        case DECREMENT:
        	retrun Object.assign({}, state, {
                number: state.number - action.diff
            });
        default:
        	retrun state;
    }
}
```

> # `Object.assign`
>
> 파라미터로 전달된 객체들을 순서대로 합친다.
>
> 역순으로 객체를 왼쪽으로 덮어쓰면서 새 객체를 만들어준다.
>
> ```javascript
> const target = { a: 1, b: 2 };
> 
> console.log(target);
> // expected output: Object { a: 1, b: 2 }
> 
> const source = { b: 4, c: 5 };
> const returnedTarget = Object.assign(target, source);
> 
> console.log(target);
> // expected output: Object { a: 1, b: 4, c: 5 }
> // assign 메서드 진행하며 이미 값이 바뀜
> console.log(returnedTarget);
> // expected output: Object { a: 1, b: 4, c: 5 }
> ```

## 스토어

### `createStore(counter)`

- 스토어 생성 메서드
- 파라미터
  - 리듀서 함수(필수)
  - 기본값(옵션)

```javascript
const store = createStore(counter);
```

### `store.subscribe(콜백함수)`

- 보통 `react-redux`의 `connect()`가 대신함

```javascript
const unsubscribe = store.subscribe(() => {
    // 스토어 상태에 변화가 일어날 때마다 실행된다.
   console.log(store.getState()); 
    // 현재 스토어 상태를 콘솔에 찍음
    // 구독을 취소하는 unsubscribe()가 반환된다.
    // 나중에 구독을 취소할 때는 unsubscribe()을 부르면 됨
});
```

