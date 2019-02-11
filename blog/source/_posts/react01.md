---
title: 리액트 인 액션 PART1
date: 2019-01-11 16:06:18
tags:
categories:
- 개발공부
- React
---

> 이 포스트는 [리액트 인 액션](https://book.naver.com/bookdb/book_detail.nhn?bid=14457098)이라는 책을 보며 혼자 공부한 기록을 남기는데 의미를 두고 있습니다.
>
> 책의 요약도 있긴 하지만 대체로 저 개인이 중요하다고 생각하는 포인트들과 이해한 영역에 대해 이야기합니다.
>
> 포스트는 언제든 삭제가 될 가능성이 있습니다....

## React의 가상 DOM

### 일단 DOM이란 뭘까

- Java Script program이 다양한 종류의 문서(HTML, XML 등)을 다루기 위한 프로그래밍 인터페이스

- XML 문서의 계층 구조를 반영한 트리 구조

  ![](https://t1.daumcdn.net/cfile/tistory/223856335971C6E023)

### 왜 가상 DOM이 필요한가

- 웹 페이지의 요소를 갱신하거나 조회할 때 사용하는 메서드나 속성
  *이를테면 getElementById, innerHTML 등*은 호스트 환경(*ex. 브라우저*)가 제공하고, Java Script는 이들을 이용해 DOM 조작
- 대형 애플리케이션의 경우 요소 갱신이 복잡해짐

### 가상 DOM의 동작 방법

- 리액트가 메모리에 가상 DOM을 생성하고 관리

- 리액트 DOM과 같은 랜더러는 가상 DOM 변경 사항을 브라우저 DOM에 반영

  (*뭔 소리야ㅠㅠㅠ, 이해 못하기 때문에 일단 써놓는다... 미래의 나 이것을 이해해서 포스트 수정해주길*)

  > **랜더러** : 코드의 결과를 브라우저에 렌더링하는 기능을 제공하는 라이브러리

### React의 가상 DOM

- 브라우저에 존재하는 문서 객체 모델(DOM)을 흉내 내거나 반영하는 데이터 구조 또는 데이터 구조의 모음
- 가상 DOM은 애플리케이션 코드와 실제 브라우저 DOM 사이에 위치하는 중간 계층의 역할을 담당

## React의 컴포넌트

### 특징

- 캡슐화
- 재사용 및 재구성 가능
- 반드시 한 번은 재사용할 컴포넌트를 만들어야 한다 (*뭔 소리죠*)

### 그러니까 간단히 설명하면

- 기능을 단위 별로 캡슐화하는 리액트의 기본 단위
- Java Script 함수 또는 클래스로, 속성들을 입력으로 받아들임
- 내부적으로 각자의 상태를 관리
- 특정 형식의 컴포넌트의 대해 생명주기 메서드 제공

> ## 웹 API(Application Programming Interface)
>
> - 소프트웨어를 개발하기 위한 루틴과 프로토콜의 집합
>
> - 프로그램이나 플랫폼과 상호작용을 하기 위해 정해진 것을 외부에 노출하는 방법

### 예제를 보자

```react
// index.js
const node = document.getElementById("root");
```

요소에 대한 참조를 해당 변수에 저장한다.

이제부터 개발할 리액트 앱은 이 DOM 내에 렌더링된다.

```html
<!-- index.html -->
<div id-"root"></div>
```

여기까지가 리액트 라이브러리 다운로드, root라는 id를 지닌 DOM 요소를 찾는 것

여기서 컴포넌트를 생성하려면  `ReactDOM.render` 메서드 실행

신텍스는 아래와 같다.

```react
ReactDOM.render(
	ReactElement element,
    DOMElement container,
    [function calllback]
)
```

`ReactDOM` 은 `ReactElement` 타입의 요소와 DOM 요소를 필요로 한다.

> ### ReactElement 요소
>
> - 경량
> - 상태 없음
> - 내부 상태 변경 불가
>
> ### ReactComponentElement
>
> - React 컴포턴트를 표현하는 함수나 클래스에 대한 참조를 의미 (*무슨 소리죠...*)
>
> ### ReactDOMElement
>
> - DOM 요소를 가상으로 표현한 객체

### `React.createElement`

`ReactElement` 를 생성할 때 쓰는 메서드

신텍스

```react
React.createElement(
    String / ReactClass type,
    // html 요소의 태그 이름을 문자열로 전달하거나 리액트 클래스 전달
    // 즉 리액트가 생성할 타입 지정
    {props:value, ...},
    // html 요소에 지정될 특성(attributes) 지정하거나
    // 컴포넌트 클래스 인스턴스에 사용할 속성 지정
	[Children], ...
    // 여기에 또 다른 자식 컴포넌트를 넣을 수 있다.
    // 컴포넌트는 중첩된다.
)
```

### 상태가 있는(stateful) 컴포넌트

#### React Class

생성방법

```react
class ReactClassName extends Component {
    // React.Component 추상 클래스를 상속하는 자바스크립트 클래스 선언
    render() {}
    // 리액트 요소를 리턴하는 render 메서드 정의
    // ver 16부터는 배열을 이용해 여러 개의 리액트 요소 리턴도 가능
    // 해당 메서드는 내부 데이터(컴포넌트에 저장된 상태)와
    // 컴포넌트 메서드
    // React.Component 추상 클래스 기반 클래스로부터 상속된 추가 메서드에도 접근 가능
    
    // React는 이런 컴포넌트에 대한 보조 인스턴스를 생성하기 때문에 저장된 상태는 컴포넌트 전체가 활용 가능
}
```

클래스를 이용해 컴포넌트를 생성하면 `props` 객체에 접근 가능

> ## `props`  객체
>
> - 컴포넌트에 전달할 수 있는 데이터
> - 컴포넌트 생성 시점에 지정도 가능
> - 컴포넌트 내부에서 수정해서는 안된다.

> ## `this`
>
> Java Script의 키워드로 컴포넌트의 인스턴스를 가리킨다.

### `PropTypes`

- 속성의 유효성 검사를 할 수 있는 것
- `prop-types` 패키지 설치 후 사용이 가능
- `React.Component` 클래스에 `popTypes` 속성 추가 필요
- 책 내에서는 propTypes의 커스터마이징까지는 다루고 있지 않아 [외부 레퍼런스](https://medium.com/@sangboaklee/react-proptypes-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0-7a0615da236) 참조

```react
import React, { Component } from "react";
import { render } from "react-dom";
// React와 React-dom 라이브러리를 가져온다.
import PropTypes from "prop-types";
// PropTypes 패키지 설치

const node = document.getElementById("root");

class Post extends Component {
  render() {
    return React.createElement(
      ...
    );
  }
}

Post.propTypes = {
    // Post라는 리액트 클래스의 props에 대한 유효성 검사
  user: PropTypes.string.isRequired,
    // user라는 props에는 String 자료형만 들어갈 수 있다.
  id: PropTypes.number.isRequired
    // id라는 props에는 number 자료형만 들어갈 수 있다.
};
// 이런 형태는 너무 기본적인 기능이기에 잘 사용되지 않는다고 함!

render(App, node);
```

이렇게도 쓸 수 있다고 한다.

```react
Post.propTypes = {
  config: PropTypes.shape({
    series: PropTypes.array.isRequired,
    xAxis: PropTypes.shape({
      categories: PropTypes.array,
    }),
  }).isRequired,
  chartType: PropTypes.arrayOf(PropTypes.string).isRequired,
  currentChartType: PropTypes.oneOf(['view', 'conversion']),
};
```

*그런데 아직 외부 레퍼런스의 모든 내용을 이해한 건 아니라서.... 점차 공부하면서 덧붙여볼 예정*



> ​	