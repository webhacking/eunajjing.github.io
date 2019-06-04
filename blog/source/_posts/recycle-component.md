---
title: 컴포넌트 재사용법
date: 2019-06-04 18:18:18
tags:
categories:
- 개발공부
- React
---

# 컴포넌트 재사용법

## 버튼 재사용법

```javascript
import React from 'react';
import styles from './Button.scss';
import classNames from 'classnames/bind';
import {Link} from 'react-router-dom';

const cx = classNames.bind('styles');

const Div = ({children, ...rest} ) => <div {...rest}>{children}</div>

const Button = ({children, to, onClick, disabled, theme = 'default'}) => {
    const Element = (to && !disabled) ? Link : Div;
    return (
        <Element
            to={to} className={cx('button', theme, {disabled})}
            onClick={disabled ? () => null : onClick}>
            {children}
        </Element>
    );
};

export default Button;
```

## 속성 재사용법

```javascript
render() {
    return (
      <MonitorCard>
        <Counter title="Success" count={this.props.success} />
        <Counter title="Failure" count={this.props.failure} color="red" />
        <Counter title="Error Rate" count={this.state.errorRate} unit="%" />
      </MonitorCard>
    );
  }
```

```javascript
import * as React from "react";
import { FormattedNumber } from "./FormattedNumber";

const DEFAULT_UNIT = "";
const DEFAULT_COLOR = "#000";

export const Counter = props => {
  const unit = props.unit || DEFAULT_UNIT;
  const color = props.color || DEFAULT_COLOR;

  return (
    <div className="item">
      <p>{props.title}</p>
      <p style={{ color }}>
        <FormattedNumber value={props.count} />
        <span className="unit">{unit}</span>
      </p>
    </div>
  );
};
```

## children을 통한 재사용

### 라우터 단

```javascript
import * as React from "react";
import { BrowserRouter, Route } from "react-router-dom";
import { DefaultLayout } from "../containers";
import * as Pages from "../pages";

interface IProps {}

const Router: React.FC<IProps> = () => {
  return (
    <BrowserRouter>
      <DefaultLayout>
        <Route exact path="/" component={Pages.Dashboard} />
        <Route exact path="/orders" component={Pages.Order} />
      </DefaultLayout>
    </BrowserRouter>
  );
};

export default Router;
```

`DefaultLayout`으로 컴포넌트를 감싼다.

```javascript
// import 구문, 타입 정의 생략

class LayoutContainer extends React.PureComponent {
  render() {
    return (
      <div>
        <Row>
          <Col>
            <Header/>
            <Sider/>
          </Col>
          <Col>
            <Drawer/>
            <Content>
              <Row>
                <Col>
                	{this.props.children}
                	{/* 여기에 컴포넌트가 주입된다 */}
                </Col>
              </Row>
            </Content>
          </Col>
        </Row>
      </div>
    );
  }
}
```

라우터를 타고 들어온 컴포넌트가 `this.props.childeren` 으로 주입된다.

### `component` 속성으로 들어온 컴포넌트

```javascript
import * as React from "react";
import { Row, Col, Card } from "antd";
import { OrderStatusContiner } from "../containers";
import { PageHeader } from "../components";

export default class Dashboard extends React.PureComponent {
  render() {
    return (
      <React.Fragment>
        <PageHeader label="대시보드" />
        <OrderStatusContiner />
      </React.Fragment>
    );
  }
}
```

위의 소스는 페이지와 같은 역할을 한다. 페이지가 많아지면 위와 같은 소스는 **만드는 페이지마다 `PageHeader`의 `label`의 값을 하나하나 넣어줘야 한다는 문제**가 있다. 만약 페이지가 많다면, `label`에 들어갈 상태를 만들어 관리하는 게 더 낫다.

> # 컴포넌트 재사용을 잘하려면
>
> 자바스크립트 ui 관련 라이브러리 소스를 열어 해당 라이브러리는 어떻게 컴포넌트를 재사용할 수 있게 만들었는지를 많이 볼 것

