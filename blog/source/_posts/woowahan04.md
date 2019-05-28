---
title: 우아한테크러닝 Typescript & React 101 04
date: 2019-05-27 12:17:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

# api

## endpoint 관리

### json 파일

- 빌드 환경을 리모트에서 받아올 수 있음
- 빌드 환경이 바뀌더라도, 약간의 수정으로 재사용 가능

### js 파일

- 관리가 더 원활하다

#### 예제

```javascript
const SERVER = "https://codebrew.kr";
const API_PREFIX = "openapi";

interface Config {
  orders: {
    request: {
      success: string;
      failure: string;
    };
  };
}

const config: Config = {
  orders: {
    request: {
      success: `${SERVER}/${API_PREFIX}/orders/request/success`,
      failure: `${SERVER}/${API_PREFIX}/orders/request/failure`
      // 도메인 별로 엔드포인트들
			// 실제로 요청하는 것들 정의
    }
  }
};

export default config;
```

## 실제로 받아와봅시다

### 핵심은, 프로미스 객체 래핑

요즘 비동기 처리를 하는 라이브러리는 자동으로 프로미스 객체로 감싸 리턴된다. 그러나 한 번 더 프로미스 객체로 래핑해 리턴하는 것을 추천하는데, 이는 나중에 타 패키지를 이용하더라도 소스를 덜 고쳐도 되어 재사용성이 높기 때문이다.

```javascript
import axios, { AxiosResponse } from "axios";
import endpoint from "./endpoint.config";

interface RequestSuccessResp {
  status: string;
}

// 상속 이용해서 그룹핑 진행
interface numberOfSuccessfulOrderResp extends RequestSuccessResp {
  result: {
    success: number;
  };
}

interface numberOfFailedOrderResp extends RequestSuccessResp {
  result: {
    failure: number;
  };
}

export function fetchNumberOfSuccessfulOrder(): Promise<
  numberOfSuccessfulOrderResp
> {
  return new Promise((resolve, reject) => {
    axios
      .get(endpoint.orders.request.success)
      .then((resp: AxiosResponse) => resolve(resp.data))
      .catch(reject);
  });
}

export function fetchNumberOfFailedOrder(): Promise<numberOfFailedOrderResp> {
  return new Promise((resolve, reject) => {
    axios
      .get(endpoint.orders.request.failure)
      .then((resp: AxiosResponse) => resolve(resp.data))
      .catch(reject);
  });
}
```

# and 연산자로 컴포넌트를 렌더하는 것이 힘들다면

## `Maybe.tsx`

```javascript
import * as React from "react";

interface IProps {
  test: boolean;
  children: React.ReactNode;
}

export const Maybe: React.FC<IProps> = ({ test, children }) => (
  <React.Fragment>{test ? children : null}</React.Fragment>
);
```

3항 연산자만을 지닌 컴포넌트를 미리 생성해둔다.

## 컴포넌트 안에서

```javascript
export interface OrderStatusProps {
  showTimeline: boolean;
  // 중략
}

class OrderStatus extends React.Component<OrderStatusProps> {
  // 중략

render() {
    return (
      <MonitorCard>
        <Counter title="Success" count={this.props.success}>
          <Maybe test={this.props.showTimeline}>
          	<TinyChart/>
          </Maybe>
				</Counter>
			</MonitorCard>
	)
}
```

이렇게 하면 test로 들어오는 값이 참일 때 하단 컴포넌트가 출력이 된다.

# 알림창의 경우 컴포넌트 위치

```javascript
import * as React from "react";
import { NotificationContainer, OrderStatusContiner, MonitorControllerContainer } from "./containers";
import { Typography } from "antd";
// css 파일 import 생략

export default class App extends React.PureComponent {
  render() {
    return (
      <div>
        <NotificationContainer />
        {/* 해당 컨테이너는 가시적인 Ui가 없으며,
        렌더 구문이 이렇게 되어 있다
        
        render() {
    			return (
      			<>
			        <div />
     				</>
    			);
  			}
        
        */}
        <header>
          <Typography.Title>React & TS Boilerplate</Typography.Title>
        </header>
        <main>
          <OrderStatusContiner />
          <MonitorControllerContainer />
        </main>
      </div>
    );
  }
}
```

> ### 라이브러리가 UI를 제공하는 경우
>
> 굳이 컴포넌트 `tsx` 파일을 생성하지 않아도 되며, 관련 사항은 라이브러리 도큐먼트를 참고한다.

## 알림을 관리하는 컨테이너의 state 구조

대부분 패턴이 있는데,

```javascript
export interface StoreState {
  monitoring: boolean;
  showTimeline: boolean;
  duration: number;
  notifications: INotification[];
  success: number;
  failure: number;
  successTimeline: ITimelineItem[];
  failureTimeline: ITimelineItem[];
}
```

```javascript
export interface INotification {
  id: number;
  type: string;
  msg: string;
  show: boolean;
  // 노출 되었는지 안 되었는지
  timestamp: number;
}

export interface ITimelineItem {
  time: string;
  count: number;
}
```

스토어에 배열로 알림에 대한 정보를 쌓고, 알림에 대한 상태를 바꿀 수 있는 액션들을 추가로 정의한다. 리듀서 쪽에서 알림의 상태를 바꾸는 액션을 디스패치 하는 식으로 제어하는 게 배운 예제의 로직.