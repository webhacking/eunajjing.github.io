---
title: Hook
date: 2019-06-01 23:03:18
tags:
categories:
- 개발공부
- React
---

# react Hooks

함수형 컴포넌트에서 가변적인 상태를 지닐 수 있게 하는 기능.

해당 함수는 함수의 제일 상단에 작성해야 한다.

## useState

```javascript
import React, {useState} from 'react';

const Counter = () => {
	// 당연히, 함수형 컴포넌트 안에 사용되어야 한다.
	const [value, setValue] = useState(0);
  // 배열이 리턴되며, 이를 비구조화 할당으로 빼온다
  // value는 상태가 되며
  // setValue는 해당 상태를 제어할 수 있는 함수이다.
  // useState에 보내는 인자는 상태의 초기 값이다.
  
  return (
    <>
    	<button onClick={() => setValue(value + 1)}>+1</button>
      <button onClick={() => setValue(value - 1)}>-1</button>
    </>
  );
}
```

## useEffect

컴포넌트 렌더링 시에만 특정 작업을 수행하도록 설정하는 함수

`componentDidMount`와 `componentDidUpdate` 를 합친 형태이다.

```javascript
useEffect(() => {
  console.log('마운트 될 때와 해당 컴포넌트가 업데이트 될 때 실행됩니다.');
});
```

만약 특정 시점에만 호출될 수 있도록 하고 싶다면,

### 마운트 시점에만 호출

```javascript
useEffect(() => {
  console.log('마운트 될 때만 실행됩니다.');
}, []);
```

### 조건 달기

```javascript
useEffect(() => {
  console.log('해당 컴포넌트가 마운트 되고, 업데이트 될 때만 실행됩니다.');
}, [component]);
```

### clean up

```javascript
useEffect(() => {
  console.log('effect');
  console.log(name);
  return () => {
    console.log('cleanup');
    console.log(name);
  };
});
```

실행 순서는 다음과 같다.

- 리턴되는 함수 우선 실행
- 그 다음 `useEffect` 리턴 구문 외 블록 실행
- 즉, 클린업 함수는 매번 실행된다

## useContext

일단 컨텍스트는 리덕스의 스토어 같은 역할로, 특정 상태를 보관한다.

### createContext

컨텍스트 생성 역할, 인자로 값을 넣어줄 수 있다.

```javascript
const testContext = createContext("blahblah")
```

### useContext

생성한 컨텍스트를 꺼내온다.

```javascript
const value = useContext(testContext);
```

> 사용하게 되면,
>
> ```javascript
> import React, { useContext, createContext } from "react";
> 
> const ContextSample = () => {
>   const ThemeContext = createContext("red");
>   const theme = useContext(ThemeContext);
> 
>   const style = {
>     width: "64px",
>     hight: "64px",
>     background: theme
>     // 여기에 컨텍스트의 값이 들어간다
>   };
>   return <div style={style} />;
> };
> 
> export default ContextSample;
> ```

## useReducer

해당 hook을 사용하게 되면, `useState` 를 사용할 필요가 없다. 실제 리듀서처럼 여러 상태를 관리할 수 있기 때문

```javascript
import React, { useReducer } from 'react';

function reducer(state, action) {
// 리듀서 함수를 만들어준다.
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 };
    case 'DECREMENT':
      return { value: state.value - 1 };
    default:
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });
  // 리듀서의 초기값과 초기 상태가 될 객체를 인자로 넣어준다.

  return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value}</b> 입니다.
      </p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+1</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-1</button>
    </div>
  );
};

export default Counter;
```

## useRef

`ref`를 만들되, 해당 `ref` 값은 함수 밖에 저장된다. `ref` 값과 렌더는 별개이다. 즉, **값이 바뀐다고 해도 렌더가 다시 일어나지 않는다.** 아래의 예시는 단순히 렌더가 새로 일어날 때마다 `ref`가 새로 만들어지는 상황 때문에 `useRef` 를 통해 ref를 생성했다.

```javascript
import React, { useState, useMemo, useRef } from "react";

const Average = () => {
  const getAverage = numbers => {
    if (numbers.length === 0) return 0;
    const sum = numbers.reduce((a, b) => a + b);
    return sum / numbers.length;
  };

  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const handleInsert = () => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");

    inputEL.current.focus();
  };

  const handleChange = e => {
    setNumber(e.target.value);
  };

  const avg = useMemo(() => {
    getAverage(list);
  }, list);
  // 마찬가지로 호출 제어

  const inputEL = useRef(null);

  return (
    <div>
      <input value={number} onChange={handleChange} ref={inputEL} />
      <button onClick={handleInsert}>추가</button>
      {list.map((value, index) => (
        <li key={index}>{value}</li>
      ))}
      <p>평균 값 : {getAverage(list)}</p>
    </div>
  );
};

export default Average;

```

## useMemo

`useCallback` 과 거의 유사하다. 차이는 리턴 값이 함수냐(`useCallback`), 리액트 노드냐의 차이다.

```javascript
const emailAccessory = useMemo(() => {
  return email !== '' && <button>X</button>
  // 리턴 값이 컴포넌트에 할당됨
}, [email])
// 두 번째 인자인 배열은 useEffect과 같은 역할
```

> ### 왜 이런 구문을 사용하나요?
>
> 자바스크립트는 객체의 값이 같더라도 다른 것으로 인지한다. 즉, 
>
> ```javascript
> const a = {
> 	aa : "aa",
> 	bb : "bb"
> }
> 
> const b = {
> 	aa : "aa",
> 	bb : "bb"
> }
> ```
>
> 객체 a와 b는 같지 않다.
>
> 퓨어 컴포넌트의 경우, 렌더 여부, 업데이트 여부를 넘어오는 프롭스가 이전과 다른지를 검사해서 처리한다. 우리 눈에는 같은 객체가 넘어갈지라도, 자바스크립트에게는 다른 것이므로 매번 렌더가 새로 실행되는 것이다. 위와 같이 `useMemo` 를 통해 넘기게 된다면, 해당 값을 같은 객체가 계속 넘어가게 되고, 불필요한 렌더가 발생하지 않는다.