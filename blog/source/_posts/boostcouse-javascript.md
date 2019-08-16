---
title: javascript
date: 2019-07-18 15:39:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# javascript

## 변수

- `var`, `let`, `const` 로 선언
- 어떤 것을 사용하는가에 의해서 scope가 다름

## 연산자

- 우선순위를 표현에는 ()를 사용

- 수학연산자는 +,-,*,/,% 등

- 논리 연산자  `===`, `!==`, `>=`,  `<=`

  - `==`
    - 자바스크립트가 묵시적으로 타입 변환을 하여 값을 비교할 가능성이 있음
  - `===`
    - 정확한 타입까지 일치하는지를 검사

- 관계 연산자  `&&`, `||`

  > ```javascript
  > const name = "kona"
  > const myname = name || "eunajjing"
  > const my-name = name && "eunajjing"
  > // 이 경우 myname, my-name은 자동으로 name의 값이 할당된다
  > // name이 null이거나 undefined일 경우 eunajjing이라는 값이 할당된다
  > ```

- 삼항 연산자 `(data > 10) ? "ok" : "no"`


## 타입

- 타입은 선언할 때가 아니고, **실행타임에 결정**

- 함수 안 파라미터나 변수는 **실행될 때 그 타입이 결정**

- 타입 체크의 또렷한 방법은 없음
  - `toString.call(값)`을 이용해서 그 결과를 매칭
  - 문자, 숫자와 같은 기본 타입은 `typeof` 키워드를 사용

  - 배열은 타입을 체크하는 `isArray()`

### 타입들

- `undefined`
- `null`
- `boolean`
- `number`
- `string`
  - `.split(":")`
    - 문자열을 파라미터로 들어온 것으로 나눈 뒤, 나뉘어진 문자를 배열로 만든다
  - `.replace(":","$")`
    - 첫 파라미터의 문자열을 두 번째 파라미터로 치환
  - `.trim()`
    - 공백 제거
- `object`
- `function`
- `array`
- `Date`
- `RegExp`

> **for문에 `length`를 쓴다는 것**
>
> - 루프를 돌 때마다 배열의 `length`를 확인하기 때문에 비효율적
> - 때문에 다른 변수에 할당한 뒤 처리하는 게 효율적

## 함수

함수는 **파라미터의 개수와 인자의 개수가 일치하지 않아도 오류가 나지 않는다**. 만약 인자의 개수를 맞추지 않을 경우 `undefined`이 값으로 들어간다.

### 함수 표현식

변수에 함수 표현식을 담아놓은 것

```javascript
const func1 = () => {
	console.log(func2);
  // fun2 = undefined
  const func2 = () => {
    return 'blahblah'
  }
}
```

- 이 경우 `func1`을 호출하면 `func2 is not a function`으로 뜰 것이다
- 아직 `func2()`에 값이 할당되기 전이므로 자동으로 해당 값은 `undefined`가 들어간다
- `undefined`는 `function`이 아니므로 함수가 아니라는 에러가 뜨는 것
- 해당 부분이 함수 표현식

```javascript
const func2 = () => {
    return 'blahblah'
}
```

- 만약 함수 선언문으로 바꾼다면 함수가 **호이스팅** 되어 함수로 인식이 된다

```javascript
const func1 = () => {
  const result = func2()
  
  function func2() {
    return 'blahblah'
  }
}
```

> **호이스팅**
>
> - 자바스크립트 함수는 실행되기 전에 함수 안에 필요한 변수값들을 미리 다 모아서 선언 
>
> - 함수 표현식과 선언문에서 다르게 동작됨을 유의
>
> > `arrow function` 의 경우 함수 표현식으로 작성되기 때문에 호이스팅 되지 않는다.

### return

- **자바스크립트의 함수는 반드시 `return` 값이 존재하며, 없을 때는 기본 반환 값인 `undefined`가 반환된다**

- 자바스크립트에는 `void`가 없다

### `arguments` 객체

- 함수가 실행되면 그 안에는 `arguments`라는 지역변수가 자동으로 생성

- 자바스크립트는 파라미터로 선언된 것보다 더 많은 인자를 보낼 수 있고, 이는 `arguments`라는 객체에 담긴다

- `arguments`의 타입은 객체

  - **배열이 아니다**

  ```javascript
  {'0': firstparam, '1' : secondparam}
  ```

  - 배열의 일부 속성을 사용할 수 없지만 아래의 것들은 사용 가능
    - `length`
    - `arguments[n]`

> 출처 : [arguments 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/arguments)

## 함수 호출 스택

### javascript의 구조

#### call Stack

- 요청이 들어올 때마다 call Stack에 새로운 프레임이 생기고, push되고 실행이 끝나면 pop된다.

#### Heap

- 동적으로 생성된 객체가 할당된다.

#### Task Queue

- 처리해야하는 Task들을 보관하는 임시 대기 큐가 있고, call Stack이 비워지면 대기열에 들어온 순서대로 실행된다
- 비동기로 호출되는 함수들은 call Stack이 아니라 task Queue에 할당된다.

> 출처
>
> - [자바스크립트의 이벤트 루프](https://asfirstalways.tistory.com/362)