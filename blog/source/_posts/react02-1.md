---
title: 리액트 인 액션 PART2 - 렌더링과 생명주기 메서드
date: 2019-01-17 00:20:18
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

# 생명 주기 메서드

이벤트 없이도 자동으로 특정 행동을 수행해야 할 경우 사용하는 것. 크게

- 컴포넌트가 동작을 시작하는 시점
- 동작 중인 시점
- 완료되는 시점

을 표기한다.

즉 클래스 기반 리액트 컴포넌트에 추가된 것으로 **컴포넌트의 생명주기 내의 특정 시점에 실행**되는 메서드

(함수형 컴포넌트는 `this.setState	` 처럼 이 생명 주기 메서드 또한 쓸 수 없다, 부모로부터 데이터를 받아 갱신할 수만 있다.)

> # 렌더링
>
> 뭔가를 만드는 것, 즉 애플리케이션을 화면에 그려내는 것
>
> 리액트는 컴포넌트를 해석해 UI로 변경시킨다.

## 분류

### 초기화

컴포넌트 클래스의 인스턴스가 생성되는 시점

- `defaultProps`
  - 부모 컴포넌트가 속성을 제공하지 않은 경우`this.props`에 기본적으로 설정되는 값
  - 컴포넌트가 마운트되기 전이어서 `this.props`를 쓸 수 없을 때 사용
  - 인스턴스가 공유하는 복합 객체를 리턴한다.
- `state` : 생성자 내에서 컴포넌트의 초기 상태를 설정하는데 사용

### 마운팅

컴포넌트가 DOM에 삽입되는 시점, 해당 메서드들은 한 번만 호출된다.

1. 부모 컴포넌트의 `defaultProps` 와 `state` 설정
2. 자식 컴포넌트의 `defaultProps` 와 `state` 설정
3. 부모 컴포넌트의 `componentWillMount()`  호출, 즉 마운트 되기 전 호출
4. 부모 컴포넌트의 `render()` 실행
5. 자식 컴포넌트의 `componentWillMount()`  호출, 즉 마운트 되기 전 호출
6. 자식 컴포넌트의 `render()` 실행
7. **마운트, DOM에 존재하며 컴포넌트에 연결된 DOM 인스턴스 생성**
8. 자식 컴포넌트의 `componentDidMount()` 실행
9. 자식 컴포넌트의 `render()` 실행
10. 부모 컴포넌트의 `componentDidMount()` 실행
11. 부모 컴포넌트의 `render()` 실행

#### `componentWillMount()`

- 최초의 렌더링이 실행되기 직전에 한 번만 호출된다.
- 마운트되기 전 상태를 결정하거나 다른 동작을 수행할 때 쓴다.
- 만약 이 메서드가 아닌 다른 곳에서 상태를 변경하면 다시 랜더링 과정이 실행된다. *잉?*

#### `componentDidMount()`

- 컴포넌트가 DOM에 삽입될 때 한 번 호출된다.
- 컴포넌트의 상태, 속성, **참조**에 접근할 수 있다.
- 응답으로 전달받은 데이터를 이용해 컴포넌트 상태를 갱신하는 작업을 주로 수행한다.
- DOM을 조작하는 서드파티 라이브러리를 호출한다.

### 갱신

컴포넌트 상태나 속성으로 전달된 새 데이터에 의해 갱신되는 시점

1. 마운팅된 부모의 컴포넌트에서 갱신 발생
2. `componentWillReceiveProps(nextProps)`  실행
3. `shouldComponentUpdate(nextProps, nextState)`가
   - true라면 **(항상 true 리턴)**
     4. `componentWillUpdate(nextProps, nextState)` 실행
     5. `render()` 실행
     6. `componentDidUpdate(prevProps, prevState)` 실행
   - false라면 (상태를 항상 불변 객체로 처리하고 `render()` 메서드 내에서 속성과 상태를 읽기 전용으로만 사용할 경우)
     4. `render()	` 를 포함한 메서드들이 실행되지 않는다.

#### `componentWillReceiveProps(nextProps)`

- `this.state`를 이용해 상태를 갱신한 후 갱신 과정에서 `render()`가 호출되기 전 상태를 전환하기 위한 용도로 활용
- 기존 속성은 `this.props`를 통해 접근
- `this.setState()`를 호출해서 렌더링이 추가적으로 실행되지는 않음

#### `shouldComponentUpdate`

- 리액트가 제공하는 메서드들로 충분하지 않은 경우에만 사용하는 게 좋다.
- 컴포넌트에 새 속성이나 상태가 전달되었을 때, 렌더링이 이루어지기 직전에 호출된다.
- 렌더링 최초에는 호출되지 않는다.

#### `componentWillUpdate(nextProps, nextState)`

- 갱신이 이루어지기 전 준비 절차를 수행할 때 사용
- `setState()`를 사용할 수 없음
- 갱신이 실행되기 직전에 호출됨
- 렌더링 최초에는 호출되지 않음

#### `componentDidUpdate(prevProps,prevState)`

- 컴포넌트가 갱신된 후 DOM을 조작할 때 사용
- 컴포넌트가 갱신된 직후 호출
- 렌더링 최초에는 호출되지 않음

예제

```react
class Child extends React.Component {
    static defaultProps = () => {
        console.log('자식 컴포턴트의 defaultProps에서 찍힌 log');
    }
    constructor(props) {
        super(props);
        console.log('자식 컴포넌트의 생성자에서 찍힌 log');
        this.state = {
            name: 'Mark'
        };
    }
    componentWillMount() {
        console.log('자식 컴포넌트의 componentWillMount()');
    }
    componentDidMount() {
        console.log('자식 컴포넌트의 componentDidMount()');
    }
    componentWillReceiveProps(nextProps) {
        console.log('자식 컴포넌트의 componentWillReceiveProps');
        console.log(nextProps);
    }
    shouldComponentUpdate(nextProps, nextState) {
        console.log('자식 컴포넌트의 shouldComponentUpdate');
        console.log(nextProps); // 부모가 던져준 속성 값
        console.log(nextState); // 자신이 가지고 있는 상태 값
        return true;
    }
    componentWillUpdate(nextProps, nextState) {
        console.log('자식 컴포넌트의 componentWillUpdate');
        console.log(nextProps);
        console.log(nextState);
    }
    componentDidUpdate(previousProps, previousState) {
        console.log('자식 컴포넌트의 componentDidUpdate');
        console.log(previousProps); // name: "" 이렇게 찍히는데 왜 그럴까
        console.log(previousState); // 자신이 가지고 있는 상태 값
    }
    render() {
        console.log('자식 컴포넌트의 render에서 찍힌 log');
        return(
            <div key="name">name: {this.props.name} </div>
        );
    }
};

class Parent extends React.Component {
    static defaultProps = () => {
        console.log('부모 컴포넌트의 defaultProps');
        return {
            true: false
        };
    }
    constructor(props) {
        super(props);
        this.state = {
            text: ''
        }
        this.onInputChange = this.onInputChange.bind(this);
    }
    componentWillMount() {
        console.log('부모 컴포넌트의 componentWillMount()');
    }
    componentDidMount() {
        console.log('부모 컴포넌트의 componentDidMount()');
    }
    onInputChange(e) {
        this.setState({text: e.target.value});
    }
    render() {
        console.log('부모 컴포넌트에서 rander에서 찍힌 Log');
        return [
            <h2 key="h2">컴포넌트의 라이프사이클을 배우자</h2>,
            <input key="input" value={this.state.text} onChange={this.onInputChange}/>,
            <Child key="Child" name={this.state.text}/>
        ]
    }
};

ReactDOM.render(<Parent/>, document.getElementById('root'));
```

콘솔

![](/images/console.png)

### 언마운팅

컴포넌트가 DOM에서 제거되는 시점

`componentWillUnmount()`로 만들 수 있다.

**`componentDidUnmount` 메서드는 존재하지 않는다.** 컴포넌트가 일단 제거되면 그 수명은 끝나기 때문에, 작업 수행이 불가하기 때문이다.

### 에러

`componentDidCatch(err, errorInfo)`를 이용해서 처리가 가능하다.

그러나 책 내 예제를 아무리 살펴봐도 에러가 나는 터라 실행을 해보진 않았다.

#### `componentDidCatch(error, errorInfo)`

- 에러가 발생한 컴포넌트 및 트리 구조 상의 하위 컴포넌트를 언마운트한다
- 생성자, 생명주기 메서드, `render()`에서 에러가 발생한 경우 호출된다



