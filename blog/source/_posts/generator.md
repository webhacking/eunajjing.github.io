---
title: Generator
date: 2019-05-22 15:46:18
tags:
categories:
- 개발공부
- javascript
---

# *Generator

## Generator function

반드시 `function*` 키워드를 필요로 한다.

```javascript
function* foo() {

}
```

여러 진입점과 제어점을 가지고 있다.

### `yield`

함수 실행 중 중단한다. 값을 넘겨줄 수도 있다.

```javascript
function* foo() {
	yield;
	yield 10;
  return 100;
}
```

해당 함수는 처음 `yield`를 만났을 때 중단되어 돌아가며, 다시 실행되었을 때는 콜러 측에서 10이라는 값을 전달 받을 수 있다.

```javascript
function* foo() {
	yield* [1, 2, 3];
  return 100;
}
```

이 경우 콜러 측에서 처음에는 1을, 다음 실행 때는 2를, 그 다음 실행 때는 3을 전달 받을 수 있다. 즉, **순회**한다.

`yield`를 이용해서 무한 루프를 돌리기도 한다. 아래의 예제는 콜러 측에서 증가하는 num을 받을 수 있다.

```javascript
let num = 0;
while(true) {
   yield ++num;
}
```

## Generator Object

제너레이터 펑션의 실행 결과로 리턴 받는 객체. 즉, **제너레이터 자체는 실행되지 않는다**. 실행하기 위해서는 `next()`를 실행해야 한다.

```javascript
const result = foo();
// result typeof Generator Object
result.next();
```

함수는 `yield`를 만나거나, `return`을 만날 때까지 실행된다. `next()`를 통해 인자를 전달할 수도 있다.

```javascript
function* foo() {
	const result = yield;
	// result is 'a'
  return 100;
}

const obj = foo();
obj.next();
// 여기까지 실행되었을 때, 아직 result의 값으로 할당이 안된 상태!
obj.next('a');
// 할당 시작되는 부분
```

### `obj.next()`

리턴되는 것 또한 객체다. 이 객체의 속성들은 이렇게 생겼다.

```javascript
obj = {
  value : '',
  // yield를 통해 전달한 값
  done : ''
  // true or false
  // 함수가 return을 만나 종료되었다면 true,
  // yield로 잠깐 멈춰진 것이라면 false
}
```

만약 `done`의 값이 `true`인데, 제너레이터 펑션을 또 `next()` 시킨다면 리턴 값으로 `undefined`가 뜬다.

### `obj.return()`

해당 함수가 `return`을 만나지 않았는데도 강제로 종료된다. 이 경우 `done`의 값이 `true`가 된다. 만약 인자값을 넣어준다면 `value`의 값에 할당하며 종료된다.

```javascript
obj.return();
// { value: undefined, done: true }
obj.return('a');
// { value: 'a', done: true }
```

### `obj.throw()`

예외처리를 위한 것으로, 제너레이터 펑션 안에 `try/catch`문이 구현되어 있을 때 `catch`문에서 잡을 수 있다.

```javascript
function* gen() {
  while(true) {
    try {
       yield 42;
    } catch(e) {
      console.log(e);
      // e가 찍힌다
    }
  }
}

const g = gen();
g.throw('e');
// 보통은 에러 객체를 생성해서 넘겨주겠지만, 예시로 문자열을 전달한다고 했을 때
```

## async / await를 굳이 Generator로 바꿔보자

### 원본

```javascript
const foo = () => {
  return new Promise((res) => {
    setTimeout(()=> {
      res(100);
    }, 1*1000)
  })
}

const foo2 = async () => {
  const result = await foo();
  console.log(result);
}

foo2();
```

### 바꾸기

```javascript
const foo = () => {
  return new Promise((res) => {
    setTimeout(()=> {
      res(100);
    }, 1*1000)
  })
}

function* main() {
  yield foo();
  ...
}

const generatorObj = main();
const it = generatorObj.next();
// yield foo(); 에서 멈춰져있고 foo()는 실행되었다.
// it의 value에 프로미스 객체가 들어와있다
it.value.then(resp=>generatorObj.next(resp));
// 그래서 이렇게 쓸 수 있다
```

#### 그러다가 잠시 찾아온 혼란

```javascript
const foo = () => {
  return new Promise((res) => {
    setTimeout(()=> {
      res(100);
    }, 1*1000)
  })
}

function* main() {
  const result = yield foo();
  console.log(result);
  return 10;
}

const generatorObj = main();
const it = generatorObj.next();
generatorObj.next();
```

예제의 마지막 `next()`가 실행이 되면 `result`로 프로미스 객체가 할당이 될테니,
콘솔에 프로미스 객체가 찍힐 줄 알았는데 현실은 `undefined`. **즉, `foo()`는 첫 `next()`에만 실행되고, 다음 `next()` 때는 실행되지 않는다.**

## redux-saga

리덕스 사가는 프로미스 객체를 리턴하는 비동기 함수, 이를 제어하는 제너레이터로 이루어져있다. 즉, 리덕스 사가는 제너레이터 객체의 넥스트로 리턴 받은 객체의 **`done` 값이 `true`가 될 때까지, `value`에 프로미스 객체가 들어와 해당 객체를 이용해 `then()`을 쓸 수 있을 때까지 계속 `next()`를 실행시키며 돌리는 구조**