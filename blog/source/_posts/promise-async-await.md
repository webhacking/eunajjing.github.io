---
title: 자바스크립트 엔진 구조, promise, async와 await
date: 2019-05-22 14:33:18
tags:
categories:
- 개발공부
- javascript
---

# 자바스크립트에서 비동기 처리하기

## 시작하기 전에, 자바스크립트 엔진의 구조

![](https://t1.daumcdn.net/cfile/tistory/225AF03B58E4956C26)

### Call Stack

- 요청이 들어올 때마다 call Stack에 새로운 프레임이 생기고, push되고 실행이 끝나면 pop된다.

### Heap

- 동적으로 생성된 객체가 할당된다.

### Task Queue

- 처리해야하는 Task들을 보관하는 임시 대기 큐가 있고, Call Stack이 비워지면 대기열에 들어온 순서대로 실행된다
- 비동기로 호출되는 함수들은 Call Stack이 아니라 task Queue에 할당된다.

## 그래서, 비동기를 어떻게

### Promise()

#### 비동기로 실행할 함수 준비

어떤 함수를 비동기적으로 실행하고자 한다면, 해당 함수는 `promise` 객체를 리턴해야 한다.

```javascript
const foo = () => {
  return new Promise((resolve, reject)=> {
    setTimeout(()=> {
      resolve(100);
    }, 1*1000);
  });
};
```

이 경우 `foo()`를 실행하면 프로미스 객체가 리턴된다. 프로미스 객체는 리턴 되기 전에 인자로 함수를 실행하며, 해당 함수는 두 개의 인자를 가진다. **만약 `resolve()`나 `reject()` 둘 중 하나가 실행되었다면, 해당 프로미스의 상태는 고정되며 차후에 실행된 상태 변경 함수는 무시된다**

-  `resolve()`

  프로미스 객체가 가진 함수가 실행되어 제대로 이행됐을 때 실행

- `reject()`

  프로미스 객체가 가진 함수가 실행되긴 했는데, 사용자가 바라지 않는 모양으로 실행되었을 때 기입해 처리. 이 경우 프로미스 객체의 상태는 계속 `pedding`으로 유지

```javascript
const p = foo();
```

이 때 `p`는 프로미스 객체가 담긴다. 만약 `resolve()`나 `reject()` 둘 다 실행이 되지 않은 프로미스 객체라면 해당 프로미스의 상태 값은 `pedding`이다.

#### 비동기로 실행해 받을 준비

프로미스 객체는 `then()`을 쓸 수 있다. `then()`의 인자 값들은 함수여야 한다.

```javascript
p.then(r => console.log(r),
      err => console.log(err));
```

- 첫번째 함수

  인자로 들어온 함수의 파라미터는 `resolve()`로 들어온 값

- 두번째 함수

  `reject()`로 들어온 값

##### 만약 두번째 함수가 없어서, `reject()`로 전달한 값을 받을 함수가 없다면?

해당 값은 계속 뒤로 밀린다. 나중에는 default 오류 처리기에 도달하는데, 해당 처리기는 이런 모양새로 생겼다.

```javascript
const errCatch = (err) => throw err;
```

그런데 이런 것까지 없다면 맨 끝의 `catch()`가 받도록 처리할 수 있다.

```javascript
p.then(r => console.log(r),
      err => console.log(err))
.catch(err=> console.log(err));
```

#### 예제

```javascript
const task1 = () => {
  return new Promise((resolve, reject)=> {
    setTimeout(()=> {
      resolve(100);
    }, 1*1000);
  });
};
const task2 = (cb) => {
	// cb로 들어오는 값은 task1에서 넘겨준 100이다
  return new Promise((resolve, reject)=> {
    setTimeout(()=> {
      resolve(200);
    }, 1*1000);
  });
};

const p2 = task1();
p2.then(r => task2(r))
	.then(x => console.log(x))
	// 여기서 찍히는 값은 task2에서 넘겨준 200이다.
	.catch(err => console.log(err));
```

#### `Promise.all([...])`

```javascript
const task1 = () => {
  return new Promise((resolve, reject)=> {
    setTimeout(()=> {
      resolve(1000);
    }, 1*1000);
  });
};
const task2 = () => {
  return new Promise((resolve, reject)=> {
    setTimeout(()=> {
      resolve(200);
    }, 1*1000);
  });
};

const p2 = task1();
const p3 = task2();

Promise.all([p2, p3]).then(r=>console.log(r));
// 배열로 프로미스 객체를 넘겨준다
// 해당 결과 값들도 then이 있어서 처리가 가능
```

해당 함수는 순차적으로 처리할, 프로미스 객체들을 배열로 넣어주어 처리한다. 해당 값들도 `then()`이 존재하기 때문에 같은 작업을 수행하도록 설정할 수 있다.

### `async/await`

#### 키워드 정리

- `async` : 비동기를 처리할 함수 앞에 붙이는 키워드
- `await` : 프로미스 객체를 리턴해주는 함수 호출문 앞에 붙이는 키워드로, `resolve/reject`가 실행될 때까지 기다린다. 리턴 값으로 `resolve()`로 던진 값을 받을 수 있다.

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
}

foo2();
```

> 제너레이터...도 써야 하는데 약간 자신이 없다...