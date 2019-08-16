---
title: WEB UI 개발
date: 2019-07-19 16:05:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# WEB UI 개발

## Window 객체

- 디폴트의 개념으로 생략이 가능

### `setTimeout`

- 인자로 `callback` 함수를 받는다
- 인자로 들어온 `callback` 함수는 비동기로 실행되기 때문에, **동기적인 실행이 끝난 뒤 실행**

## `DOM`과 `querySelector`

### `DOM`(Document Object Model)

![](https://cphinf.pstatic.net/mooc/20180126_280/1516956194218XFPk5_PNG/2-2-2_Dom_tree.png)

- 브라우저에서는 `HTML`을 `DOM`이라는 객체형태의 모델(`DOM Tree`)로 저장
- `DOM Tree`를 탐색하기 위해 `JavaScript`로 탐색알고리즘을 구현하는 것은 비효율적
  - 브라우저에서는 `DOM`이라는 개념을 통해 다양한 `DOM API` 제공

#### `getElementById()`

- 특정 `Element`가 반환되어 특정 정보들을 뽑아낼 수 있다
  - `id`
  - `className`
  - `style`
- 특정 `Element`가 반환되어 특정 정보들을 대입할 수 있다

#### `Element.querySelector()`

- `CSS Selector` 문법을 활용해 `DOM`에 접근
- 최초로 발견한 `node`만을 반환

##### `Element.querySelectorAll()`

- 배열 반환

## Event

### 이벤트 등록

```javascript
const element = document.querySelector("#id");
element.addEventListener("click", function(e){
  // e는 이벤트 객체
	// 이벤트 핸들러 / 이벤트 리스너 기술
}, false);
```

### 이벤트 객체

- `브라우저`는 `이벤트 리스너`를 호출하면

- 사용자로부터 어떤 이벤트가 발생했는지에 대한 정보를 담은 `이벤트 객체`를 생성해서 `리스너 함수`에 전달

#### `event.target`

- 이벤트가 발생한 `element`를 가리킴
- `DOM Node`이므로,  매한가지로 특정 정보를 뽑아내거나 추가할 수 있다

## Ajax

- XMLHTTPRequest 통신
- **비동기 로직**

```javascript
function ajax(data) {
 const oReq = new XMLHttpRequest();
 oReq.addEventListener("load", function() {
   console.log(this.responseText);
   // this는 oReq
 });    
 oReq.open("GET", "http://www.example.org/getData?data=data");//parameter를 붙여서 보낼수있음
 oReq.send();
}
```

### `open()`

- 요청의 준비

### `send()`

- 서버로 발송

### `load()`

- 응답이 오면 발생하는 이벤트
- 콜백 함수 실행
  - 이 때의 `ajax`는 반환되어 콜스택에서 사라짐

## 디버깅

- `Pause` OR `Continue`
  - 평소에는 `Pause`, 브렉포인트가 잡힌 상태에선 `Continue`
  - 다른 브레이크포인트가 잡힐 때까지 코드를 진행
- `Step over next function call` 
  - 스텝 오버는 코드 라인을 한 스탭 진행하는데 현재 실행 라인에 함수 실행 코드가 있다면 함수는 실행하는데 함수 안의 코드로는 진입하지 않는다
  - 즉 라인의 함수를 실행만 한다
- `Step into next function call`
  - 스텝 인투는 스텝 오버와 다르게 현재 실행 라인의 코드에 함수가 있다면 함수 안의 첫 번째 코드로 진입해 들어가 다시 하나씩 라인별로 코드를 실행
- `Step out of current function`
  - 스텝 인투로 들어온 함수를 끝까지 실행하고 밖으로 빠져나와 해당 함수를 실행한 함수로 돌아간다
- `Active` / `Deactive breakpoint` 
  - 브레이크포인트를 끄거나 켬
- `Pause on exception`
  - 예외가 발생하면 해당 위치에 브레이크포인트를 잡아준다

### 디버깅하기

- 코드 일정 부분에 브레이크 포인트를 잡는다
- 웹 어플리케이션을 동작한다
  - 화면에 `Paused in debugger	`가 출력된다
  - 개발자도구 우측에는 어떤 포인트에서 멈춰졌는지 표기된다

- 만약 특정 라인까지 실행을 원한다면 `script execution` 버튼 클릭