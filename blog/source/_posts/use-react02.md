---
title: 리액트를 다루는 기술02
date: 2019-02-17 16:51:18
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

# `ref`

html에서 `id`를 사용해 DOM에 이름을 다는 것처럼, 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법

DOM을 꼭 직접적으로 건드려야 할 때 쓰인다.

> 리액트 컴포넌트 안에서도 `id`를 사용할 수 있지만, 특수한 경우가 아니면 권장되지 않음
>
> 같은 컴포넌트를 여러 번 사용할 때, id가 유일하지 못하기 때문
>
> `ref`는 컴포넌트 내부에서만 작동하기 때문에 전역적이 아님

syntax

`<input ref={(ref) => {this.input=ref}}/>`

```javascript
class ValidationSample extends Component {
	handleButtonClick = () => {
        this.setState({
            clicked: true,
            validated: this.state.password === '0000'
        });
        this.password.focus();
        // this.password는 ref가 password인 컴포넌트를 부르는 것
    }
    render() {
        return (
            <div>
                <input type="password" name="password" onChange={this.handleChange}
                className={this.state.validated ? 'success' : 'failure'} 
                ref={(ref) => this.password=ref} />
                {*/이 때 ref는 password가 된다
                 만약 다른 이름으로 지정하고 싶다면
                 this.하고_싶은_이름 = ref
                 /*}
                <button onClick={this.handleButtonClick}>검증하기</button>
            </div>
        );
    }
}
```

컴포넌트 자체에도 `ref`를 붙일 수 있고, 이 `ref`를 이용해 컴포넌트 내부 메서드 및 멤버 변수에도 접근이 가능하다.

> # ES6의 비구조화 할당 문법
>
> 객체에서 특정 값을 추출해 따로 레퍼런스를 만들 때 유용
>
> ```javascript
> const object = {
>     foo: 1,
>     bar: 2
> }
> 
> const {foo, bar} = object;
> console.log(foo, bar);
> 
> function print({foo, bar}) {
>     console.log(foo, bar);
> }
> 
> print(object);
> ```
>
> [외부 레퍼런스](https://pro-self-studier.tistory.com/31)
>
> # 전개 연산자 `...`
>
> [외부 레퍼런스](https://blog.hanumoka.net/2018/09/17/javascript-20180917-javascript-spread-operator/)
>
> ```javascript
> function test(a, b, c) {}
> let x = [1, 2, 'd'];
> text(...x);
> ```
>
> 이 때 `test`의 파라미터로 1, 2, d가 간다.
>
> x는 배열이다.
>
> 즉, 배열의 내용을 복사하되 껍데기인 `[]`를 지운다.

```javascript
// ScrollBox.js
class ScrollBox extends Component {
  scrollToBottom = () => {
    const { scrollHeight, clientHeight } = this.box;
    this.box.scrollTop = scrollHeight - clientHeight;
  }

  render() {..}
}

export default ScrollBox;

// app.js
import ScrollBox from './ScrollBox';

class App extends Component {
  render() {
    return (
      <div>
        <ScrollBox ref={(ref) => this.scrollBox=ref}/>
        <button onClick={() => this.scrollBox.scrollToBottom()}>밑으로</button>
		// 컴포넌트가 렌더링될 때, 아직 자식 컴포넌트는 렌더링되기 전이므로
		// 새로운 함수를 만들어 실행하는 것
      </div>
    );
  }
}
```

# `map()`

syntax : `arr.map(callback, [thisArg])`

- `callback` : 새로운 배열 요소를 생성하는 함수
  - 파라미터들
    - `currentValue` : 현재 처리하고 있는 요소
    - `index` : 현재 처리하고 있는 요소의 index 값
    - `array` : 현재 처리하고 있는 원본 배열
- `thisArg` : 옵션 항목. 콜백 함수 내부에서 사용할 this 레퍼런스

```javascript
class IterationSample extends Component {
    render() {
        const names = ['눈사람', '얼음', '눈', '바람'];
        const nameList = names.map((name) => (<li>{name}</li>));
        return (
            <ul>
                {nameList}
            </ul>
        );
    }
}
```

## `key`

컴포넌트 배열을 렌더링했을 때 어떤 원소에 변동이 있었는지 알아내기 위해 사용

위의 소스는 실행되지만, 콘솔에는 `key`가 없다는 에러가 난다.

```javascript
class IterationSample extends Component {
    render() {
        const names = ['눈사람', '얼음', '눈', '바람'];
        const nameList = names.map((name, index) => <li key={index}>{name}</li>);
        return (
            <ul>
                {nameList}
            </ul>
        );
    }
}
```

## `concat()`

```javascript
class IterationSample extends Component {
    state = {
        names: ['눈사람', '얼음', '눈', '바람'],
        name: ''
    }
    handleChange = (e) => {
        this.setState({
            name: e.target.value
        });
    }
    handleInsert = () => {
        this.setState({
            names: this.state.names.concat(this.state.name),
            name: ''
        });
    }
    render() {
        const nameList = this.state.names.map((name, index)
                                              => <li key={index}>{name}</li>);
        return (
            <div>
                <div>
                    <input onChange={this.handleChange} value={this.state.name}/>
                    <button onClick={this.handleInsert}>추가</button>
                </div>
                 <ul>
                  {nameList}
                 </ul>
             </div>
        );
    }
}
```

## `slice()`

어떤 배열의 `begin`부터 `end`까지(`end` 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환

```javascript
var animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]
```

- 숫자는 0부터 시작한다.

- end는 그냥 -1로 기억하자.

  (begin부터 end-1까지 반환된다)

```javascript
class IterationSample extends Component {
	handleRemove = (index) => {
        const {names} = this.state;
        this.setState({
            names: [...names.slice(0, index), ...names.slice(index+1)]
        })
    }
    render() {
        const nameList = this.state.names.map((name, index) => 
        <li key={index} onClick={() => this.handleRemove(index)}>
            {name}
        </li>);
        return ()
    }
}
```

## `filter()`

이렇게 romove를 구현할 수도 있다.

```javascript
class IterationSample extends Component {
	handleRemove = (index) => {
        const {names} = this.state;
        this.setState({
            names: names.filter((item, i) => {
                // item에는 각각의 값이 오고
                // (사용하지 않는 파라미터라 삭제하려고 했는데
                // 삭제 후 동작이 안되는 걸 보니 옵셔널이 아닌 것 같다)
                // i는 인덱스
                return i !== index
                // filter는 배열을 리턴한다.
            })
        })
    }
}
```

​	

# 라이프 사이클

```javascript
class LifeCycleSample extends Component {
  state = {
    number: 0,
    color: null,
  }

  myRef = null; // ref 를 설정 할 부분
  
  
  constructor(props) {
    super(props);
    console.log('constructor');
  }

  static getDerivedStateFromProps(nextProps, prevState) {
      // nextProps : 부모가 전달한 값
      // prevState : 내가 가지고 있는 값
    console.log('getDerivedStateFromProps');
    if (nextProps.color !== prevState.color) {
      return { color: nextProps.color };
    }
    return null;
  }

  componentDidMount() {
    console.log('componentDidMount');
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log('shouldComponentUpdate');
    // 숫자의 마지막 자리가 4면 리렌더링하지 않습니다
    return nextState.number % 10 !== 4;
  }

  componentWillUnmount() {
    console.log('componentWillUnmount');
  }
  
  handleClick = () => {
    this.setState({
      number: this.state.number + 1
    });
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('getSnapshotBeforeUpdate');
    // 그 전에 부모가 내게 줬던 값과 내가 가지고 있던 상태 값
    if (prevProps.color !== this.props.color) {
      return this.myRef.style.color;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('componentDidUpdate');
    if (snapshot) {
      console.log('업데이트 되기 직전 색상: ', snapshot);
    }
  }

  render() {
    console.log('render');
    
    const style = {
      color: this.props.color
    };

    return (
      <div>
        <h1 style={style} ref={ref => this.myRef = ref}>
          {this.state.number}
        </h1>
        <p>color: {this.state.color}</p>
        <button onClick={this.handleClick}>
          더하기
        </button>
      </div>
    )
  }
}
```

