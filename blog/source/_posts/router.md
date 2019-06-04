---
title: 우아한테크러닝 Typescript & React 101 05 router
date: 2019-06-04 12:17:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

# SPA의 라우팅

spa에서 라우팅 하는 방법은 매번 바뀌는데, 수업에서는 크게 두 가지가 소개되었다.

## #(hash mark) 사용

```html
<a href="#blahblah">blahblah로 이동</a>
```

`#`은 기본적으로 클라이언트 스팩이므로 서버에 요청이 가지 않는다. 즉, 페이지 갱신이 되지 않는다. 웹 페이지 내부의 이동이기 때문이다. javascript 단에서는 `hashChange` 이벤트를 사용해서 hash가 변경됨을 감지하고, 필요한 비동기 처리를 수행한다. 

## pushState(PJAX) 이용

```html
<a href="/blahblah">blahblah로 이동</a>
```

웹서버로 해당 url이 전달되긴 하나, 서버는 단 하나의 엔트리 포인트만 클라이언트로 전달한다. 클라이언트 단에서는 path를 사용하여 비동기 처리를 수행한다. 리액트 단에서 사용할 라우팅의 방법이다.

> 추가 출처 : [SPA&ROUTING](https://poiemaweb.com/js-spa)

# 그래서, 해봅시다

```javascript
import * as React from "react";
import { BrowserRouter, Route, Switch } from "react-router-dom";
import { DefaultLayout } from "../containers";
import * as Pages from "../pages";

interface IProps {}

const Router: React.FC<IProps> = () => {
  return (
    <BrowserRouter>
      {/* 루트를 감싸 하위 요소가 전부 라우팅할 수 있도록 한다. */}
      <Switch>
        <DefaultLayout>
          <Switch>
            <Route exact path="/" component={Pages.Dashboard} />
            <PrivateRoute exact path="/orders" page={Pages.Order} />
            <Route component={Pages.PageNotFound} />
          </Switch>
        </DefaultLayout>
      </Switch>
    </BrowserRouter>
  );
};

export default Router;
```

**프로바이더 바로 밑에 `BrowserRouter`가 들어간다**고 생각하면 된다.

- `exact` : `exact={true}` 와 같은 것, 경로가 완벽히 일치했을 때 해당 컴포넌트가 보이도록 한다. 만약 해당 처리를 해주지 않을 경우, `path` 속성에 기입한 값을 포함하는 url이면 전부 렌더링한다.
- `component`: 렌더링할 컴포넌트

## 왜 이중 `Switch` 문이 필요한가

### `Switch`의 역할

- 라우팅
  - 원래 리액트 라우터는 모든 요소를 다 렌더하는 성격이 있다.
  - 만약 `Switch` 로 감싸지 않았다면, 404 페이지까지 모두 렌더한다.
  - `Switch` 안의 요소는 하나라도 매칭이 될 경우 하위 컴포넌트 렌더를 캔슬한다.
- 하위 요소에게 라우팅 경로 관련 정보를 주입
  - 해당 주입은 자식 요소에게만 전달될 뿐, 자손 요소까지 닿지 않는다.
  - 즉, 이중 `Switch`가 쓰이지 않으면 `DefaultLayout` 의 입장에서는 라우팅 정보를 받을 수 없다는 문제 발생

## `PrivateRoute`

직접 만든 컴포넌트로, 들어오는 인자의 타입이 `IProps & IStateToProps & RouteProps`, 즉 세 개를 합친 형태이다.

### 타입

#### `IProps`

##### 실제 렌더링할 컴포넌트의 타입

```typescript
type RoutePageComponent =
  | React.ComponentType<RouteComponentProps<any>>
  | React.ComponentType<any>;
```

실제 렌더링할 컴포넌트를 받는데 사용할 타입을 명명하는데,

- 라우터의 속성으로 컴포넌트가 들어오거나
- 리액트의 컴포넌트가 들어오거나

이므로 `OR` 연산자를 이용하여 타입을 정의한다. 만약 들어오는 컴포넌트가 리액트 컴포넌트라면 JSX 문법으로 받아올 수 있다. `<Page/>`

> 그런데 어차피 라우터 컴포넌트에 대한 속성은 `RouteProps` 가 가져오기도 하므로 `React.ComponentType<RouteComponentProps<any>>` 을 없애도 정상적으로 동작하더라...

```typescript
interface IProps {
  page: RoutePageComponent;
}
```

해당 타입을 이용해 `IProps` 인터페이스를 만든다.

#### `IStateToProps`

store에 들어갈 `IAuthentication` 상태와 타입이 같다. 해당 상태는 토큰을 가지고 있는데, 이 상태 값은 토큰이 있는지 여부를 검사한다.

```typescript
interface IStateToProps {
  authentication: IAuthentication;
}
// IAuthentication
// export interface IAuthentication {
//  token: string | null;
// }
```

#### `RouteProps`

라우터의 `component` 속성을 통해 들어온 컴포넌트

### 그래서 예제

```javascript
import * as React from "react";
import { IStoreState, IAuthentication } from "../store";
import { Route, Redirect, RouteProps, RouteComponentProps } from "react-router-dom";
import { connect } from "react-redux";

// 타입 정의 생략

const PrivateRouter: React.FC<IProps & IStateToProps & RouteProps> = props => {

  const Page: RoutePageComponent = props.page;
  const { authentication } = props;

  return (
    <Route
      {...props}
      render={props => {
        if (authentication) {
          return <Page {...props} />;
          // 여기에 page라는 속성으로 전달한 컴포넌트가 들어와 렌더됨
        } else {
          return (
            <Redirect
              to={{
                pathname: "/login",
                state: { from: props.location }
              }}
            />
          );
        }
      }}
    />
  );
};

const mapStateToProps = (
  state: IStoreState,
  ownProps: IProps
): IProps & IStateToProps => ({
  ...state,
  ...ownProps
});

export default connect(mapStateToProps)(PrivateRouter);
```

#### 리액트에서 컴포넌트를 주입하는 방법

- `component` 속성에 컴포넌트를 넣는다
- `children` 을 이용한다
- `render`를 함수로 구현한 뒤, 부모 컴포넌트에서 받은 `props` 와 매칭한 것만을 렌더한다.
  - 위의 예제에서 사용한 방법
  - props를 펼치기 쉽다는 장점이 있다.

#### `<Redirect>` 의 요소

- `to` : 어디로 향할 것인지, 해당 값은 객체로 들어가야 한다.
  - `pathname` : url
  - `state` : pushState에 주입할 객체
    - `from` : 원래 어디로 향하고자 했는지, 만약 로그인이 성공하면 어디로 갈 것인지

> **왜 이런 방식을 쓰는가**
>
> 리액트 라우터는 pushState의 큐에 있는 값으로 동작하기 때문에, 클라이언트의 location 객체에 직접 값을 넣으면 꼬일 가능성이 있다. redirect 컴포넌트를 리턴하게 되면 리액트 라우터가 해당 컴포넌트의 속성으로 담긴 정보를 보고 자동으로 처리한다.

## `mapStateToProps` 의 인자**들**

일반적으로 `state` 만 받는데, `mapStateToProps`에서 받을 수 있는 인자는 두 개다.

- state

- 컨테이너 속성으로 기술된 것

  (ex : exact, path="/orders")

넘어오는 컴포넌트가 컨테이너 속성을 지닌 것이라면 분명 속성이 있고 해당 값이 있을 것, `props.children` 에게 값을 전달하기 위해 가져온다. (*물론 위의 예제에서는 쓰이지 않는다*)

## 멀티 레이아웃의 구현

멀티 레이아웃을 구현하는 방법은 여러 개이다.

```javascript
// import 구문 생략

interface IProps {
  children?: React.ReactNode;
}

const Router: React.FC<IProps> = () => {
  return (
    <BrowserRouter>
      <NotificationContainer />
      <Switch>
        <Route exact path="/login">
          <FullSizeLayout>
            <Pages.Login />
          </FullSizeLayout>
        </Route>
        {/* 일반 라우터 */}
        <DefaultLayout>
          {/* url이 로그인이 아니라면 */}
          <Switch>
            {/* 다 프라이빗 라우터 */}
            <PrivateRoute exact path="/" page={Pages.Dashboard} />
            <PrivateRoute exact path="/orders" page={Pages.Order} />
            <Route component={Pages.PageNotFound} />
          </Switch>
        </DefaultLayout>
      </Switch>
    </BrowserRouter>
  );
};

export default Router;
```

