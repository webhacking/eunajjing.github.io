---
title: 컴포넌트 재사용법
date: 2019-05-15 11:45:18
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

> # 컴포넌트 재사용을 잘하려면
>
> 자바스크립트 ui 관련 라이브러리 소스를 열어 해당 라이브러리는 어떻게 컴포넌트를 재사용할 수 있게 만들었는지를 많이 볼 것

