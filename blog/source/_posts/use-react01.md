---
title: 리액트를 다루는 기술01
date: 2019-02-17 14:31:18
tags:
categories:
- 개발공부
- React
---

> 이 포스트는 [리액트를 다루는 기술](https://book.naver.com/bookdb/book_detail.nhn?bid=13799583)이라는 책을 보며 혼자 공부한 기록을 남기는데 의미를 두고 있습니다.
>
> 책의 요약도 있긴 하지만 대체로 저 개인이 중요하다고 생각하는 포인트들과 이해한 영역에 대해 이야기합니다.
>
> 포스트는 언제든 삭제가 될 가능성이 있습니다....

# 렌더링

사용자 화면에 뷰를 보여주는 것

- 초기 렌더링 : 리액트 컴포넌트가 최초로 실행하는 렌더링
- 리렌더링 :  컴포넌트의 데이터 변경으로 재실행되는 렌더링

> # 용어 정리
>
> - **빌드** : 여러 파일로 분리된 프로젝트를 한 파일로 합친다.
> - **번들링** : 파일을 묶듯이 연결한다.

## React는 왜 가상 DOM을 만드는가

실제 DOM은 변화가 일어나면 웹 브라우저가 `css`를 다시 연산하고, 레이아웃을 구성하고, 페이지를 리페인트한다.

즉, **시간이 허비된다.**

리액트는 가상 DOM 방식을 사용하여 DOM 업데이트를 추상화하고, DOM 처리 횟수를 최소화해 효율적으로 진행한다.

## 실제 DOM이 업데이트되는 과정

- 데이터 업데이트
- 전체 UI를 가상 DOM에 리렌더링
- 이전 가상 DOM과 리렌더링한 가상 DOM 비교
- 바뀐 부분만 실제 DOM에 적용

# 컴포넌트 `props`의 전달

**`<MyComponent name={3}/>`**

문자열 종류 외의 값을 전달할 때는 `{}`로 감싸야 한다. 

## `propTypes`의 종류

- `array`
- `bool`
- `func`
- `number`
- `object`
- `string`
- `symbol` : ES6에 추가된 것으로 유일한 식별자의 역할을 한다. [외부 레퍼런스](https://includestdio.tistory.com/27)
- `node` : 렌더링할 수 있는 모든 것
- `element` : 리액트 요소
- `instanceOf(MyClass)` : 특정 클래스의 인스턴스
- `oneOf(['~', '~'])` : 주어진 배열 요소 중 하나의 값
- `oneOfType([React.PropTypes.~, React.PropTypes.~])` : 주어진 배열 안의 종류 중 하나
- `arrayOf(React.PropTypes.~)` : 주어진 종류로 구성된 배열
- `objectOf(React.PropTypes.~)` : 주어진 종류의 값을 가진 객체
- `shape({ ~: React.PropTypes.~, ~: React.PropTypes.~})` : 주어진 스키마를 가진 객체
- `any` : 아무 종류

#  `state`의 사용

```javascript
class MyComponent extends Component {
    constructor(props) {
        super(props);
        this.state = {
            number: 0
        }
    }
    // 만약 생성자를 만들지 않는다면 Component 클래스의 생성자 메서드를 사용한다.
    // 생성자를 사용해 추가 작업을 하려면, 메서드 내부에서 부모 클래스인
    // Component의 생성자를 호출해야하고,
    // 이를 위해 super()를 사용
    // 컴포넌트를 만들 때 props의 값들을 사용할 것이므로 메서드의 파라미터로 보낸다.
    render() {
        return(
            <div>
                안뇽, 내 이름은 {this.props.name} 이얌!
            </div>
        )
    }
}
```

굳이 생성자에서 하지 않아도 되긴 한다.

```javascript
class MyComponent extends Component {
    state = {
        number: 0
    }
}
```

## `setState`의 사용

```javascript
this.setState({
    수정할 필드명: 값,
    수정할 필드명: 값
})
```

> # ES6의 화살표 함수
>
> ```javascript
> function BlackDog() {
>     this.name = '흰둥';
>     return {
>         name: '검둥',
>         bark: function() {
>             console.log(this.name + '멍멍');
>         }
>     }
> }
> 
> function WhiteDog() {
>     this.name = '흰둥';
>     return {
>         name: '검둥',
>         bark: () => {
>             console.log(this.name + '멍멍');
>         }
>     }
> }
> 
> const blackDog = new BlackDog();
> blackDog.bark();
> // 검둥 : 멍멍
> const whiteDog = new WhiteDog();
> whiteDog.bark();
> // 흰둥 : 멍멍
> ```
>
> 일반 함수는 자신이 종속된 객체를 `this`로 가리키나, 화살표 함수는 자신이 종속된 인스턴스를 가리킨다.
>
> ```javascript
> const test = (value) => value*3
> ```
>
> 따로 `{}`를 하지 않으면 연산 값을 리턴하겠다는 뜻

# 메서드의 바인딩

컴포넌트의 생성자에서 각 메서드를 this와 바인딩하는 작업이 필요하나,

바벨의 `transform-class-properties` 문법을 사용해 간단하게 화살표 함수로 구현이 가능

```javascript
class EventPractice extends Component {
    state = {
        message: ''
    }
    handleChange = (e) => {
        this.setState({
            message: e.target.value
        })
    }
    handleClick = () => {
        console.log(this.state.message);
    }
    render() {
        return (
            <div>
                <h2>이벤트 연습</h2>
                <input type="text" name="message"
                onChange={this.handleChange}/>
                <button onClick={this.handleClick}>확인</button>
            </div>
        );
    }
}
```

​	

# `e.target.name`

해당 input의 name을 가리키는 이벤트 객체의 값

이를 이용해서 이렇게 핸들링도 가능하다.

```javascript
    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
        })
    }
```

​	

# `onKeyPress` 핸들링

```javascript
import React, { Component } from 'react';

class EventPractice extends Component {
    state = {
        username: '',
        message: ''
    }
    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
        })
    }
    handleClick = () => {
        console.log(this.state.username + ":" + this.state.message);
    }
    handleKeyPress = (e) => {
        if (e.key === 'Enter') {
            // 만약 키가 엔터이면
            this.handleClick();
        }
    }
    render() {
        return (
            <div>
                <h2>이벤트 연습</h2>
                <input type="text" name="message"
                onChange={this.handleChange} onKeyPress={this.handleKeyPress}/>
                <button onClick={this.handleClick}>확인</button>
            </div>
        );
    }
}

export default EventPractice;
```