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
> - React 컴포넌트를 표현하는 함수나 클래스에 대한 참조를 의미
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

### React의 상태

상태 : 어느 특정한 시점의 어떤 것에 대한 정보

- `this.state` : 변경 가능(mutable) 상태와
- `this.props` : 변경 불가능한(immutable) 상태

로 나뉜다.

- 리액트에서 쓰이는 `컴포넌트` 는 `React.Component` 클래스를 확장한 자바스크립트 클래스로 정의하며 가변(state)/불변(props) 상태를 모두 관리할 수 있다.
- 반면 함수를 이용해 생성한 컴포넌트(immutable)는 불변 상태(props)만을 관리할 수 있다.

#### 상태가 있는(stateful) 컴포넌트

##### React Class

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

###### 기본 상태의 설정

```react
class CreateComment extends Component {
  constructor(props) {
    super(props);
    this.state = {
      content: "",
      user: ""
    };
      // 생성자 안에서 super 함수를 호출해 클래스 인스턴스 state 속성에 기본 상태 객체를 대입
  }
  render() {
    return React.createElement(
      "form",
      {
        className: "createComment"
      },
      React.createElement("input", {
        type: "text",
        placeholder: "Your name",
        value: this.state.user
          // 이렇게 상태에 접근
      })
    );
  }
}
```

**`this.state`의 속성은 직접 덮어쓸 수 없다**

**`this.setState`** 메서드를 호출해야 한다.

```react
setState(function(preState, props) -> nextState,
    callback
) -> void
```

갱신 함수를 매개변수로 전달받아 다시 지원 인스턴스를 갱신한 후, 새로운 값을 DOM에 적용한다는 특징이 있다.

리액트가 상태를 일괄적으로 변경하기에, `setState` 메서드를 호출한다고 해서 그 결과가 곧바로 적용되지 않는다.

리액트는 사용자 입력에 대한 응답으로 갱신이 이루어진다.

##### React의 이벤트

가상 DOM을 구현하는 것의 일부로 모의(synthetic) 이벤트 시스템을 구현한다.

브라우저에서 이벤트가 발생하면, 리액트 애플리케이션 이벤트로 변환한다.

자바스크립트가 이벤트를 처리하는 방법대로, 브라우저에서 발생하는 이벤트에 반응할 이벤트 핸들러를 설정하면 된다.

**리액트의 이벤트핸들러는 리액트 요소나 컴포넌트 자체에 설정된다.**

때문에 이렇게 설정된 이벤트가 전달하는 데이터를 이용해 컴포넌트 상태를 갱신할 수도 있다.

```react
class Post extends Component {
  constructor(props) {
    super(props);
  }
  render() {
    return React.createElement(
      "div",
      {
        className: "post"
      },
      React.createElement(
        "h2",
        {
          className: "postAuthor",
          id: this.props.id
        },
        this.props.user,
        React.createElement(
          "span",
          {
            className: "postBody"
          },
          this.props.content
        ),
        this.props.children
      )
    );
  }
}

class Comment extends Component {
  constructor(props) {
    super(props);
  }
  render() {
    return React.createElement(
      "div",
      {
        className: "comment"
      },
      React.createElement(
        "h2",
        {
          className: "commentAuthor"
        },
        this.props.user,
        React.createElement(
          "span",
          {
            className: "commentContent"
          },
          this.props.content
        )
      )
    );
  }
}

class CreateComment extends Component {
  constructor(props) {
    super(props);
    this.state = {
      content: "",
      user: ""
    };
    this.handleUserChange = this.handleUserChange.bind(this);
    this.handleTextChange = this.handleTextChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
      // 클래스로 생성한 컴포넌트는 메서드를 자동으로 바인딩하지 않는다.
      // 때문에 생성자 내에서 이 메서드들을 직접 바인딩해줘야 한다.
      // 이전 버전의 리액트의 경우 메서드가 자동 바인딩되었으나, 자바스크립트 클래스로 변환되며 직접 해줘야 함
  }
  handleUserChange(event) {
    const val = event.target.value;
      // val에는 input에 사용자가 입력한 텍스트가 들어온다.
    this.setState(() => ({
      user: val
    }));
  }
  handleTextChange(event) {
    const val = event.target.value;
    this.setState({
      content: val
    });
  }
  handleSubmit(event) {
    event.preventDefault();
    this.props.onCommentSubmit({
      user: this.state.user.trim(),
      content: this.state.content.trim()
    });
    this.setState(() => ({
      user: "",
      content: ""
    }));
  }
  render() {
    return React.createElement(
      "form",
      {
        className: "createComment",
        onSubmit: this.handleSubmit
      },
      React.createElement("input", {
        type: "text",
        placeholder: "Your name",
        value: this.state.user,
        onChange: this.handleUserChange
      }),
      React.createElement("input", {
        type: "text",
        placeholder: "Thoughts?",
        value: this.state.content,
        onChange: this.handleTextChange
      }),
      React.createElement("input", {
        type: "submit",
        value: "Post"
      })
    );
  }
}

class CommentBox extends Component {
  constructor(props) {
    super(props);
    this.state = {
      comments: this.props.comments
      // 부모에게 comments를 보낸다
    };
    this.handleCommentSubmit = this.handleCommentSubmit.bind(this);
  }
  handleCommentSubmit(comment) {
    // 제일 큰 특징은 복사본을 생성해서 대입한다는 것
    const comments = this.state.comments;
    comment.id = Date.now();
    const newComments = comments.concat([comment]);
    this.setState({
      comments: newComments
    });
  }
  render() {
    return React.createElement(
      "div",
      {
        className: "commentBox"
      },
      React.createElement(Post, {
        id: this.props.post.id,
        content: this.props.post.content,
        user: this.props.post.user
      }),
      this.state.comments.map(function(comment) {
        return React.createElement(Comment, {
          key: comment.id,
          id: comment.id,
          content: comment.content,
          user: comment.user
        });
        // map 메서드를 이용해 각 댓글에 대응하는 리액트 요소를 생성해 리턴한다.
      }),
      React.createElement(CreateComment, {
        onCommentSubmit: this.handleCommentSubmit
        // CreateComment에게 handleCommentSubmit 메서드 전달
      })
    );
  }
}

render(
  React.createElement(CommentBox, {
    comments: data.comments,
    post: data.post
  }),
  node
);
```

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

## JSX

반드시 **바벨**과 같은 JSX 전처리기 프로그램이 필요하다.

> 전처리기 프로그램 : JSX 코드를 자바스크립트 코드로 바꾸는 것

### 특징

- 특성 표현식

  `<User a = "this.props.b"/>`가 아니라, **`<User a = {this.props.b}/>`**

- 불리언 특성

  `<Planactive/>`, `<Input checked/>` 의 경우 리액트에서 true 값이다. 만약 false를 주고 싶다면

  `attribute={false}` 같은 특성 표현식이 필요하다.

- 중첩 표현식

  요소 내 표현식의 값을 추가할 때도 `<p>{this.props.content}</p>`로 써주어야 한다.

