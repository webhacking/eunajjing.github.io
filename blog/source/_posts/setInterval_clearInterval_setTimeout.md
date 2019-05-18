---
title: setInterval, clearInterval, setTimeout
date: 2019-05-18 15:51:18
tags:
categories:
- 개발공부
- javascript
---

# `setInterval()`, `clearInterval()`, `setTimeout()`

> - 토이 프로젝트를 하다가 아무 생각 없이 `setTimeout()`으로 특정 기능을 구현하려고 했는데, 하다보니 제어가 힘들었다.
> - 방법을 고심하다가, 어떤 블로그에서 세 개의 함수를 비교한 걸 보게 되었고, 나의 요구사항에는 `setInterval()`로 구현하는 게 더 쉽다는 걸 알게 되었다.

## `setTimeout()`

일정 시간 후에 특정 동작을 실행한다

```javascript
setTimeout(callbackFunc(), 1 * 1000);
```

## `setInterval()`

일정 시간 마다 특정 동작을 실행한다.

```javascript
setInterval(callbackFunc(), 1 * 1000);
```

콜백함수 인자 대신 실행 구문을 넣어도 되긴 하지만, 한 번만 실행됨을 유의한다.

## `clearInterval()`

`setInterval()`로 반복하고 있는 걸 멈추게 한다.

```javascript
const setIntervalFunc = setTimeout(callbackFunc(), 1 * 1000);
clearInterval(setIntervalFunc());
```

