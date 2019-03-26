---
title: 리액트를 다루는 기술07
date: 2019-03-26 11:30:18
tags:
categories:
- 개발공부
- React
---

# `react-router`

![](data:image/png;base64,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)

`yarn add react-router-dom`

## `NODE_PATH` 설정

`package.json`에서 노드 패스를 설정해준다.

```javascript
"scripts": {
    "start": "NODE_PATH=src react-scripts start",
    "build": "NODE_PATH=src react-scripts build"
    (...)
}
```

window의 경우, `cross-env` 의존설정을 해준다.

```javascript
"scripts": {
    "start": "cross-env NODE_PATH=src react-scripts start",
    "build": "cross-env NODE_PATH=src react-scripts build"
	(...)
}
```

최상위 컴포넌트를 `Root.js`로 바꾸고, 기존 `App.js`를 감싼다.

```javascript
import React from 'react';
import {BrowserRouter} from 'react-router-dom';
import App from './App';

const Root = () => {
    return (
        <BrowserRouter>
            <App />
        </BrowserRouter>
    );
};

export default Root;
```

`App.js`에서 라우터 설정

```javascript
import React from 'react';
import {Route} from 'react-router-dom';
import {Home, About} from 'pages';
/*
pages/index.js에서는
export {default as Home} from './Home';
export {default as About} from './About';
처리
*/

const App = () => {
  return (
    <div>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};

export default App;
```

## `exact` 키워드

`path`가 정확하게 일치할 때 보여주기 위해 사용.

만약 위의 코드에서 `exact`을 사용하지 않았더라면, 모든 페이지는 `/`이 포함되므로 모두 다 보였을 것

## 라우트 경로에 특정 값 넣기

### params

```javascript
import React from 'react';
import {Route} from 'react-router-dom';
import {Home, About} from 'pages';

const App = () => {
  return (
    <div>
      <Route exact path="/" component={Home} />
      <Route path="/about/:name?" component={About} />
    </div>
  );
};

export default App;
```

```javascript
import React from 'react';

const About = ({match}) => {
    return (
        <div>
            <h2>소개</h2>
            <p>
                안녕하세요, 저는 {match.params.name}입니다.
            </p>
        </div>
    );
};

export default About;
```

만약 `localhost:3000/about/react`로 접속했다면,

안녕하세요, 저는 react입니다. 라는 글자가 출력될 것

`localhost:3000/about`만 친다면 저는 입니다, 만 나온다.

### Query String

`yarn add query-string`

**라우트 내부에서 설정해준다**

**해당 값들은 전부 `String` 형태의 값임에 유의한다**

```javascript
import React from 'react';
import queryString from 'query-string';

const About = ({location, match}) => {
    const query = queryString.parse(location.search);
    const {color} = query;
    return (
        <div>
            <h2 style={{color}}>소개</h2>
            <p>
                안녕하세요, 저는 {match.params.name}입니다.
            </p>
        </div>
    );
};

export default About;
```

만약 url을 `localhost:3000/about/react?color=red`라고 타이핑해 접근한다면,

콘솔에는 `color`라는 키와 그 키 값으로 `red`가 들어있는 객체가 찍힌다.

위의 소스는 이를 이용해서 style을 정의한 것

## 라우트 이동

애플리케이션에서 다른 라우트로 이동할 때는, `a` 링크를 사용하면 페이지를 새로고침하면서 로딩하기 때문에 사용이 불가하다.

때문에 리액트 라우터 안에 있는 `Link` 컴포넌트들을 이용해야 한다.

### `Link`

```javascript
import React from 'react';
import {Link} from 'react-router-dom';

const Menu = () => {
    return (
        <div>
            <ul>
                <li><Link to="/">홈</Link></li>
                <li><Link to="/about">소개</Link></li>
                <li><Link to="/about/react">React 소개</Link></li>
            </ul>
        </div>
    );
};

export default Menu;
```

```javascript
import React from 'react';
import {Route} from 'react-router-dom';
import {Home, About} from 'pages';
import Menu from 'components/Menu';

const App = () => {
  return (
    <div>
      <Menu/>
      <Route exact path="/" component={Home} />
      <Route path="/about/:name?" component={About} />
    </div>
  );
};

export default App;
```

### `NavLink`

`Link`와 거의 유사한 기능이지만, 현재 주소와 해당 컴포넌트의 목적지 주소가 일치한다면 특정 스타일 또는 클래스를 지정할 수 있다. 즉 **active** 표시 가능

```javascript
import React from 'react';
import {NavLink} from 'react-router-dom';

const Menu = () => {
    const activeStyle = {
        color : 'green',
        fontSize: '2rem'
    };
    return (
        <div>
            <ul>
                <li><NavLink exact to="/" activeStyle={activeStyle}>홈</NavLink></li>
                <li><NavLink exact to="/about" activeStyle={activeStyle}>소개</NavLink></li>
                <li><NavLink to="/about/react" activeStyle={activeStyle}>React 소개</NavLink></li>
            </ul>
        </div>
    );
};

export default Menu;
```

`CSS`를 적용하길 원한다면 `activeClassName` 값을 지정해준다.

### javascript를 이용한 이동

로그인 같이 특정 경로로 이동을 시켜줘야 할 때 쓰는 방법

```javascript
import React from 'react';

const Home = ({ history }) => {
    return (
        <div>
            <h2>홈</h2>
            <button onClick={() => {
                history.push('/about/javascript')
            }}>
                이동
            </button>
        </div>
    );
};

export default Home;
```

### 라우트 안의 라우트

```javascript
// 안의 라우트
import React from 'react';

const Post = ({ match }) => {
    return (
        <p>
            포스트 #{match.params.id}
        </p>
    );
};

export default Post;
```

```javascript
// 밖의 라우트
import React from 'react';
import {Post} from 'pages';
import {Link, Route} from 'react-router-dom';

const Posts = ({match}) => {
    return (
        <div>
            <h3>포스트 목록</h3>
            <ul>
                <li><Link to={`${match.url}/1`}>포스트 #1</Link></li>
                <li><Link to={`${match.url}/2`}>포스트 #2</Link></li>
                <li><Link to={`${match.url}/3`}>포스트 #3</Link></li>
            </ul>
            <Route exact path={match.url} render={()=>(<p>포스트를 선택하세요</p>)}/>
            <Route exact path={`${match.url}/:id`} component={Post}/>
        </div>
    );
};

export default Posts;
```

### `location`, `match`, `history`

#### `location`

```json
{
    "pathname" : "/posts/3",
    "search" : "",
    "hash" : "",
    "key" : "xmsczi"
}
```

`search`  값에서 URL Query를 읽는데 사용하거나 주소를 바뀐 것을 감지하는데 사용

```javascript
componentDidUpdate(prevProps, prevState) {
    if(prevProps.location !== this.props.location) {
        
    } 
}
```

#### `match`

`Route` 컴포넌트에서 설정한 `path`와 관련된 데이터들을 조회할 때 사용

주로 `prams`를 조회하거나 서브 라우트를 만들 때 현재 `path`를 참조하는데 사용

url이 같아도 다른 라우트에서 사용된 `match`는 다른 정보를 알려준다.

```json
{
    isExact : true,
    params: {
        id : "2"
    },
    path: "/posts:id",
    url: "/posts/2",
   	(...)
}
```

#### `history`

현재 라우터를 조작할 때 사용

- `push('~')` : 페이지 방문 기록 남음, 
- `replace(~)` : 페이지 방문 기록 남지 않음, 페이지 이동 후 뒤로가기 버튼을 누르면 방금 전의 페이지가 아니라 방금 전의 전 페이지가 나타남

