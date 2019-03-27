---
title: react-loadable
date: 2019-03-27 17:16:18
tags:
categories:
- 개발공부
- React
---

# [react-loadable](https://github.com/jamiebuilds/react-loadable#avoiding-flash-of-loading-component)

`yarn add react-loadable`

## 장점

- 페이지 로딩을 할 때부터 청크파일들을 다른 자바스크립트파일들과 동일한 시점에서 로딩을 시작 할 수 있다.

- 서버사이드 렌더링과 함께 쉽게 코드 스플리팅을 할 수 있다.

- 로딩 중일 때 렌더링할 컴포넌트를 따로 지정할 수 있다.

  ```javascript
  import React from 'react';
  import Loadable from 'react-loadable';
  
  const Loading = () => {
    return <div>로딩중...</div>;
  };
  
  export const Home = Loadable({
    loader: () => import('./Home'),
    loading: Loading
  });
  export const About = Loadable({
    loader: () => import('./About'),
    loading: Loading
  });
  ```

- Preloading

  - 렌더링되지 않아도 특정 함수를 호출하여 미리 불러온다.
  - 링크 호버 시에 로딩을 시작한다

  ```javascript
  import React, { Component } from 'react';
  import { Route, Link } from 'react-router-dom';
  import { About, Home } from './pages';
  
  class App extends Component {
    handleMouseOver = () => {
      About.preload();
    };
    render() {
      return (
        <div>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about" onMouseOver={this.handleMouseOver}>
                About
              </Link>
            </li>
          </ul>
          <hr />
          <Route exact path="/" component={Home} />
          <Route path="/about" component={About} />
        </div>
      );
    }
  }
  
  export default App;
  ```

- [서버사이드렌더링과 함께 쓰는 방법](https://velog.io/@velopert/react-code-splitting#7.-react-loadable-%EC%9D%80-%EC%84%9C%EB%B2%84%EC%82%AC%EC%9D%B4%EB%93%9C-%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%84-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%95%98%EA%B8%B8%EB%9E%98)