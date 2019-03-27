---
title: 리액트를 다루는 기술08
date: 2019-03-27 09:44:18
tags:
categories:
- 개발공부
- React
---

# 코드 스플리팅

코드 스플리팅 :

코드를 분할한다. `webpack`에서 프로젝트를 번들링할 때 파일 하나가 아니라 파일 여러 개로 분리시켜서 결과물을 만든다. 즉 해당 파일을 필요한 시점에 불러올 수 있다.

## 청크(`chunk`) 생성

특정 파일을 비동기적으로 불러올 때

```javascript
import React, { Component } from 'react';

class AsyncSplitMe extends Component {
    state = {
        SplitMe : null
    }

    loadSplitMe = () => {
        // 비동기적으로 코드를 불러온다. 함수는 Promise를 결과로 반환한다.
        import('./SplitMe').then(({default: SplitMe}) => {
            /* import는 모듈의 전체 네임스페이스를 불러오므로
            default를 직접 지정해야 한다. */
            this.setState({
                SplitMe
            });
        });
    }

    render() {
        const {SplitMe} = this.state;
        return SplitMe ? <SplitMe/> : <button onClick={this.loadSplitMe}>로딩</button>
    }
}

export default AsyncSplitMe;
```

위의 코드가 로딩되면, 웹브라우저 개발자도구 네트워크 탭에서 `chunk.js`가 생성된 것을 확인할 수 있다.

## 라우트 코드 스플리팅

비동기적으로 불러올 코드가 많으면 청크를 생성할 때마다 파일에 비슷한 코드들을 반복하여 작성해야 한다. 조금 더 편하게 구현하기 위해 이를 함수화한다.

```javascript
import React from 'react';

export default function asyncComponent(getComponent) {
    // getComponent = import('~')
    return class AsyncComponent extends React.Component {
        static Component = null;
        state = {Component : AsyncComponent.Component};

        constructor(props) {
            // props = import('~')
            super(props);
            if(AsyncComponent.Component) return;
            getComponent().then(({default: Component}) => {
                AsyncComponent.Component = Component;
                this.setState({Component});
                /*
                props로 들어온 컴포넌트는
                해당 컴포넌트가 언마운트 후
                다시 마운트 될 때 불러오지 않고,
                static 값으로 남아있는 이전 컴포넌트로 재사용한다.
                */
            });
        }

        render() {
            const {Component} = this.state
            if(Component) {
                return <Component {...this.props} />
            }
            return null;
        }
    }
}
```

```javascript
// index.js
import asyncComponent from 'lib/asyncComponent';

export const Home = asyncComponent(()=> import('./Home'));
export const About = asyncComponent(()=> import('./About'));
export const Post = asyncComponent(()=> import('./Post'));
export const Posts = asyncComponent(()=> import('./Posts'));
```

