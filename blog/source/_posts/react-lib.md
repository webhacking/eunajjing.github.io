---
title: 리액트의 라이브러리
date: 2019-04-06 15:13:18
tags:
categories:
- 개발공부
- React
---

# react 라이브러리 정리

## CSS

### node-sass

`Sass`를 CSS로 변환

> # SCSS의 문법
>
> - 변수의 사용 가능
> - `mixin` 생성 가능
>   - 재사용되는 스타일 블록을 함수처럼 사용 가능
> - `$.자식요소` 사용 가능
>   - 부모의 요소와 자식의 요소가 함께 사용됐을 때
> - `@import` 구문 사용 가능

### open-color

`Sass` 라이브러리 중 하나로 색상 팔레트임

사용을 위해선 `@import '~open-color/open-color';`

### include-media

`Sass` 라이브러리 중 하나로 반응형 디자인을 쉽게 만들어줌

사용을 위해선 `@import '~include-media/dist/include-media';` 구문 필요

```scss
@include media(">phone", "<=tablet") {
width: 50%;
}
```

### classnames

CSS 클래스를 조건부로 설정하거나 여러 종류의 파라미터를 조합해 CSS 클래스를 설정할 수 있음

`classNames.bind()` 사용 가능

### styled-components

자바스크립트 파일안에 스타일까지 작성 가능

```javascript
const Box = styled.div`
  /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
  background: ${props => props.color || 'blue'};
  padding: 1rem;
  display: flex;
`;
```

## redux

### redux

상태 관리 로직을 컴포넌트 밖에서 관리하는 라이브러리

- `createStore(리듀서)`
- `combineReducers({객체})`
  - 서브 리듀서들을 하나로 합쳐 통합 리듀서를 만들어주는 역할
- `bindActionCreators(액션 생성 함수가 있는 객체 모듈, dispatch)`
  - 액션 생성함수 자동 바인딩
- `applyMiddkeware()`

### react-redux

- `Provider`
  - 리액트 어플리케이션에 손쉽게 스토어를 연동할 수 있도록 돕는 컴포넌트
- `connect(stateToProps, dispatchToProps)(뷰 컴포넌트)`
  - 뷰단 컴포넌트를 어플리케이션의 데이터 레이어와 묶는다

### react-actions

- `createAction(액션정의)`
- `handleActions(액션에 따라 실행할 함수들이 담긴 객체, 기본 값)`
  - 리듀서 만들 때 활용

## middleware

### redux-logger

### redux-thunk

객체가 아닌 함수도 디스패치할 수 있게 하는 미들웨어

### axios

요청의 응답 정보를 지닌 객체를 반환

### redux-promise-middleware

Promise 객체를 payload로 전달하면 요청을 시작, 성공, 실패할 때 액션의 뒷부분에 `_PENDING`, `_FULFILLED`, `_REJECTED`를 붙여서 반환

- `createPromise()`

### redux-pender

액션 객체 안 `payload`가 `Promise` 형태라면 액션의 뒷부분에 관련 접미사를 붙여 반환(`redux-promise-middleware` 와 유사하지만 객체가 온다는 게 좀 다름)

- `penderMiddleware()`
- `pender()`
  - `…pender({ type: 액션타입명, 함수 : (state, action) => {} })`
  -  type에 접미사를 붙인 액션 핸들러들이 담긴 객체를 생성
- `applyPenders()`
  - `…pender()` 를 여러 번 생성해야할 때

## react-router

### react-router-dom

- `Route`
- `Link`
- `NavLink`

### query-string

### cross-env

- window의 경우 node_path를 쓰기 위함

## ETC

### immutable

- Map
- fromJS
- List

## react-loadable

페이지 로딩을 할 때부터 청크파일들을 다른 자바스크립트파일들과 동일한 시점에서 로딩을 시작 할 수 있다.

- `Loadable({})`
- `preload()`