---
title: 우아한테크코스 Typescript & React 101 01
date: 2019-05-15 11:47:18
tags:
categories:
- 개발공부
- 우아한테크코스(React+Typescript)
---

# 구구절절

## 나는 어쩌다가 이 강의를 듣게 되었나

이 시리즈는 내가 어떤 공부를 했다, 의 기록 겸 느낀점 기록에 치중될 예정이다. 나는 프로 구구절절러이기 때문에(*...*) 이 과정을 어떻게 입과(*라는 용어가 합당한가?*)하게 되었는지도 쓸 것.

![](/images/kakaocapture.jpeg)

시작은 단순히 친한 언니가 이런 과정이 있다더라, 라고 소개해준 것. 당시 나는 리액트로 당장 프로젝트 구현은 해야하는데, 아무 것도 모르는 상황이라 닥치는대로 리액트 정보를 흡수 중에 있었다. 지인들에게 힘든 티를 팍팍 냈더니(*죄송*) 건너서 이런 식으로도 정보가 왔다. 문제는 이런 강의가 다 그렇듯 공급에 비해 수요가 너무 많다는 것. 이 소식을 듣자마자 신청했지만, 되리라는 생각은 별로 안했다.

![](/images/emailCapture.png)

그런데 운이 좋게도 당첨(*로또는 아니지만 당첨이란 말이 어울린다고 생각한다! sns에 안됐다는 사람 꽤 봐서*)이 되어 손 꼽아 해당 강좌를 기다렸고, 어제가 해당 첫날이었다.

우형 사무실이 얼마나 좋은지는 굳이 쓸 필요 없을 거 같아서... 쓰지 않는다...

## 이 과정의 타겟은 누구인가

알고 보니, 해당 과정은 내부 개발자 교육을 하기 위해 만들어졌다고 했다. 그런데 겸사겸사 외부 개발자들도 참여할 수 있게 열었고, 100여명을 선발했으며 50%는 우형 개발자라고 했다. 즉 **프론트엔드 개발자는 아닌데, 당장 뭔가를 만들어야 하는 백엔드 & 앱 개발자**를 대상으로 한다고. 하지만 외부에서 온 개발자의 90%는 프론트엔드를 전문으로 하시는 분들이 뽑히셨다고 했다.

**그래서** 해당 강좌는 자바스크립트에 나름 익숙하며, dom을 다룬 적이 있는 사람을 대상으로 하는 것 같았다.

> 와중에 듣다가 우형에서 프론트만 전문적으로 하시는 분이 20여명 계신다고 듣게 됐다. 수가 적은 터라 프론트 개발자가 필요한 모든 곳에 가 개발할 수 없다보니, 다른 분야를 하시던 분들이 프론트를 만들 수밖에 없는 상황이 되었다고. 라인 프론트 데브에서는(*이거 후기 언제 쓰냐*) 라인 내 프론트 개발자가 30여명이라고 했는데, 둘 다 큰 회사인데도 전문적인 프론트 엔지니어 수가 생각보다 적어서 놀랐다.
>
> 라인 데브 후에 네트워킹에도 참여했다. 참여자 대부분이 각 회사에서 프론트 단만을 전문적으로 개발하시는 분들이었다. 그런데 네트워킹 주최하신 분이 "다들 회사에서 프론트 혼자 하시거나 되게 소수시지 않냐~"라는 식으로 말을 꺼냈던 게 기억이 났다.
>
> 프론트가 수요는 있는데... 전문적으로 하시는 분들은 많지 않다는, 어디선가 들은 이야기가 생각 났다.

## 나는 왜 현재 이 강의를 듣는가

그동안 진짜 농담이 아니라 리액트만 했다. (*DB와 스프링이 아득하다*) 스스로 현재 상태를 가늠하기엔 예쁜 마크업은 자신이 없는데, 어떻게든 리액트로 무언가를 완성할 수 있는 상태...인 것 같다. 그래서 리액트 기초를 듣는 것도 좋지만, 알고 있는 것과 모르는 것을 구분하고, 모르지만 개발을 하기 위해선 알아야 하는 것들을 배우는 것도 좋을 것 같았다. 그럼에도 불구하고 나는 리액트 기초 강의를 현재 두 개나 듣고 있고, 하나는 심지어 유료 강의다.

그래서 주변에서... 왜 굳이 교육을 듣냐고, 이제 공식 문서를 보고 포폴용 프로젝트 개발하면 되는 거 아니냐고 하시는 분들도 많은데(*영어 못해...!*) 성향 때문인 것 같다. 어쨌든 나는 교육을 듣고 있을 때 제일 열심히 하는 사람이니까. 옆에서 누군가가 하고 있어야 부담스러워하면서(*하기 싫어하면서*) 나도 하는 사람이라, 원동력이 필요했다. 스터디도 들어봤지만, 스터디도 너무 케바케라서 모르는 게 생기면 바로바로 질문할 수 있는 교육을 더 선호하는 것 같다. 어쨌든 기초도 이해는 하지만 계속 까먹는 걸...

이 강의에서 내가 얻고자 하는 것은 아래와 같다.

- 타입스크립트와 리액트의 혼용은 어떻게 하냐
- 그래서 리액트로 뭘 어떻게 구현하는지 (*프론트에 들어가는 비지니스 로직 및 Ui 라이브러리 보통 뭐 쓰시는지*)
- 리덕스 개념 다시 잡기
- 자바스크립트 개념 다시 잡기

> 자바 스크립트는 정말 해도 해도 늘... 어려운 것 같다...
>
> 어제 우형 측에서 자바스크립트 기초 강의를 할 수는 없는 노릇이니, 링크 하나 주시면서 필요한 분들은 이걸 보는 것도 좋을 것 같다고 하셨다. 그거 가지구 모르는 거 생기면 읽으며 공부할 예정 (근데 해당 링크 공유해도 되는 건진 모르겠어서 기록하진 않는다)

# 시작

## 보일러 플레이트로 만든 `creact react-app`

만들 어플리케이션은 모두 타입스크립트 기반으로 만들기 때문에 `create react-app` 때 옵션 값인 `--typescript`를 써줘야 한다.

즉 **`yarn create react-app projectName --typescript`**

아, 나처럼 `projectName`을 명령어의 일환으로 볼 사람이 있을까 덧붙인다(*왜 그랬어 과거의 나*) `projectName`은 본인이 사용하고 싶은 프로젝트 제목을 넣어주면 된다.

오 **그 전에**

- vsCode
- node.js
- npx or yarn

을 깔아주어야 함. Yarn과 npx는 하는 패키지 관리자로 역할이 거의 같은데, 많은 예제에서 Yarn을 쓰시는데다가 더 빠르다는 이야기를 들어서 나는 계속 yarn을 썼다. 어떤 패키지 관리자를 쓰느냐에 따라 명령어에 조금씩 차이가 있다.

이렇게 프로젝트를 만들면

![](/images/woowahan01/01.png)

이런 파일 구조가 생긴다. 보일러 플레이트로 생성한 프로젝트 구조의 경우, 바꾸려면 꽤 까다롭다. 특히 `src` 폴더를 바꾸고자 하는 움직임이 좀 있었다고 들었는데(*이를테면 이름이라던가*) 페이스북 측에서 들어주지 않는다고 했다.

순수한 `create react-app`과 조금 다른 점은, **확장자와 `tsconfig.json`**이다.

### `tsconfig.json`

타입스크립트 컨파일하기 위한 설정들이 담긴 파일이라고 한다.

### `.tsx`

`ts`는 `js`와 같은 타입스크립트 코드로만 이루어진 경우에 붙고, 여기에 `x`가 붙으면 ui 컴포넌트라는 뜻이 된다.

### 리액트와 달리 설정해줘야 하는 것

- 타입스크립트는 자바스크립트의 객체/변수들에 타입을 명명해주는 게 큰 특징이다.

- 때문에 패키지들 또한 타입이 있어야 한다.

- 리액트는 타입스크립트 쪽에서 가지고 있는 저장소에 타입이 지정되어있다.

- 해당 리액트 타입의 경우 디폴트 익스포트가 없다.

- 때문에 따로 수정을 해줘야 한다.

  아래는 수정 전. `create react-app`으로 구현한 것과 소스가 일치한다.

  ```javascript
  import React from 'react';
  import ReactDOM from 'react-dom';
  ```

  아래는 수정 후, 디폴트 익스포트가 없기 때문에 모든 것을 가져오는 `*` 키워드를 써주고, alias를 붙여준다.
  **안 붙여주면 안된다!** 이렇게 가져온 모듈은 객체처럼 쓸 수 있다 `moduleName.blahblah()`

  ```javascript
  import * as React from 'react';
  import * as ReactDOM from 'react-dom';
  ```

### 그 외 리액트와 비슷한 것

- from 뒤의 패스가 상대경로이거나 이름만 덜렁 있는 경우가 있다.
- 이름만 있는 경우에는 패키지 관리자를 통해 가져온 모듈이며 해당 모듈은 `node_moudules`라는 폴더에 있다
- 익스포트는 여러 개를 할 수도 있고 다른 파일에서 임포트할 때 디폴트 모듈이 온다.

> - **예전부터 궁금했다, entry point인 `index.js`의 `recat` 모듈!**
>
> `ReactDOM`을 사용해서 `render`하긴 하는데, 도대체 `react` 모듈은 어디에 쓰이나.
>
> 가상 DOM을 만들 적 내부적으로 사용된다고. 때문에 모두 리액트 모듈을 포함해야 한다.
>
> - **이미지 파일이나 css 파일은 js 파일이 아닌데 어떻게 Js와 해서 쓸 수 있나**
>
> 번들러가 합쳐주는 역할을 수행한다. 그래서 이렇게 쓸 수 있는 것
>
> ```javascript
> import logo from './logo.svg';
> import './App.css';
> ```

## 컴포넌트를 만드는 방법

크게 3가지이다.

- 일반 컴포넌트

- 퓨어 컴포넌트

  일반 컴포넌트의 기능 중 많이 쓰는 기능만을 가지고 있으며 상대적으로 가볍다

- 함수형 컴포넌트

  화면을 그리는데 사용하며, 라이프사이클이 없다.

## `index.tsx`

```javascript
import * as React from "react";
import { render } from "react-dom";
// react-dom 모듈 중 특정한 것만 가지고 올 때 이렇게 쓴다.
// 여러 개 가지고 올 때는 콤마로 구분
import App from "./App";

const rootElement: HTMLElement = document.getElementById("root");
// : ~ 는 타입을 기술한 것
// 타입스크립트는 값을 추론해서 타입을 넣어주기 때문에 안 써도 되는 경우가 있긴 한데
// 함수의 리턴 타입을 명시하지 않았을 경우 이렇게 받을 때 명시하기도 함
// any라는 타입은 다 사용 가능하다 (그럼 왜 타입스크립트를 쓰는 걸까... 자바스크립트 화)

// 사실 위의 코드에서 해당 타입은 안 써도 된다.

render(<App />, rootElement);
```

## 보일러 플레이트를 만들어보아요!

![완성](/images/woowahan01/02.png)

### 구조

![구조](/images/woowahan01/03.png)

컴포넌트는 크게 버튼, 카드로 구성되어 있고 두 개의 컴포넌트를 감싸고 있는 부모 컴포넌트 `App.tsx`가 있는 형태

#### 버튼.js

```javascript
import * as React from "react";
import { Button } from "antd";
// 버튼 ui 가지고 옴

interface PlayButtonProps {
  // 타입을 정의하기 위해 인터페이스 사용
  monitoring: boolean;
  onPlay?: () => void;
  onPause?: () => void;
  // 위의 인터페이스는 세 개의 프로퍼티를 받는다.
  // 프로퍼티의 타입 또한 지정할 수 있으며
  // 이들은 객체가 아니기 때문에 세미콜론으로 끝을 맺는다.
  // ?는 프로퍼티가 옵션이라는 뜻
}

export const PlayButton: React.FC<PlayButtonProps> = props => {
  // React.FC 리액트의 함수 컴포넌트이며
  // 해당 함수 컴포넌트의 제네릭타입은 위에 기술한 PlayButtonProps 인터페이스이다.

  const [isPlay, togglePlay] = React.useState(props.monitoring);
  // 이건 나중에 설명한다고 했는데...
  const renderIcon = isPlay ? "pause" : "caret-right";

  return (
    <div>
      <Button
        style={{ marginTop: 20 }}
        shape="circle"
        icon={renderIcon}
				// 여기까지는 ui 라이브러리 도큐먼트를 그대로 따른 것
        onClick={() => {
          if (isPlay) {
            props.onPause && props.onPause();
            // and 연산자
            // 처음이 참(존재하고)이며 뒤가 함수이면 함수 실행
          } else {
            props.onPlay && props.onPlay();
          }

          togglePlay(!isPlay);
          // 훅인데 지금은 설명하지 않음
        }}
      />
    </div>
  );
};
```

##### 타입을 정의하는 방식

- 인터페이스 사용
- 타입 정의

> 보통은 인터페이스를 많이 사용한다고 한다.
>
> 개인적으로 그랲큐엘과 자바 두 개 다 다뤄봐서 그런지 둘 다 익숙한 방법이었다.

#### 카드

```javascript
import * as React from "react";
import { Card } from "antd";
// 마찬가지로 ui 모듈

interface OrderStatusProps {
  success: number;
  failure: number;
}

export const MonitorCard: React.FC<OrderStatusProps> = props => {
  const errorRate: string =
    // 해당 결과 값은 string 타입이다
    props.failure > 0
  	// 실패 결과가 0건 이상이면
      ? Number((props.failure / props.success) * 100).toFixed(2)
      // 실패율을 구한다, 소수점 두 자리까지 자르고 리턴
      : "0";
  const formattedNumber = (value: number): string =>
  // 프로퍼티 앞에 바로 : 찍고 타입을 기입하면 해당 함수의 리턴 타입 대한 정의을 뜻한다.
  // :string이므로 해당 함수는 String 자료형을 리턴한다.
    String(value).replace(/(\d)(?=(?:\d{3})+(?!\d))/g, "$1,");
		// 1000단위마다 콤마를 찍는다
  return (
    <Card
      bordered={false}
      bodyStyle={{
        background: "#fff",
        padding: "24px"
      }}
      // 속성의 value를 줄 때 문자열은 ""에 싸서 주면 되고,
      // 그 외의 값, 자바스크립트의 value를 줄 때는 {} 안에 기술
      // {}를 두 번 연속해서 사용한 건 객체가 value일 때
    >
      <div className="wrapper">
        <div className="item">
          <p>Success</p>
          <p style={{ color: "#000" }}>
            <span>{formattedNumber(props.success)}</span>
          </p>
        </div>

        <div className="item">
          <p>Failure</p>
          <p style={{ color: "#000" }}>
            <span>{formattedNumber(props.failure)}</span>
          </p>
        </div>

        <div className="item">
          <p>Error Rate</p>
          <p style={{ color: "#000" }}>
            <span>{errorRate}</span>
            <span className="unit">%</span>
          </p>
        </div>
      </div>
    </Card>
  );
};

```

#### `index.js`

컴포넌트들을 익스포트한다.

```javascript
export * from "./MonitorCard";
export * from "./PlayButton";
```

#### `app.tsx`

```javascript
import * as React from "react";
import { MonitorCard, PlayButton } from "./components";
// 폴더명을 경로로 줬기 때문에 자연스럽게 index.js를 불러오게 되고,
// index.js에서 컴포넌트들을 미리 익스포트 했기 때문에 쉽게 사용 가능하다.
import { Typography } from "antd";
// ui 라이브러리를 쓰기 위해 이렇게 사용

import "antd/dist/antd.css";
import "./sass/main.scss";

interface Application {
  timerId: number;
  state: {
    success: number;
    failure: number;
  };
  onStart(): void;
  onStop(): void;
}

export default class App extends React.Component implements Application {
  // class이므로 인터페이스를 상속 받아서 써도 된다.
  // 제네릭으로 해도 됨
  timerId: number = 0;
  // 위에 정의되어 있기 때문에 기술할 필요는 없음

	// class field 문법으로 실행 블록인 것인 양 써도 된다.
  // 실제로 timerId는 인스턴스의 멤버로 들어간다 (스태틱이 아님)

  state = {
    success: 0,
    failure: 0
  };

  onStart = () => {
    this.timerId = setInterval(() => {
      this.setState({
        success: this.state.success + Math.floor(Math.random() * (100 - 1) + 1),
        failure: this.state.failure + Math.floor(Math.random() * 2 - 0)
      });
      // 상태가 바뀌면 렌더 함수가 호출됨
    }, 200);
  };

  // 애로우 펑션의 this는 항상 해당 클래스의 인스턴스로 고정된다.

  onStop = () => {
    clearInterval(this.timerId);
    this.timerId = 0;
  };

  render() {
    return (
      <div>
        <header>
          <Typography.Title>React & TS Boilerplate</Typography.Title>
          <Typography>Order Monitor</Typography>
        </header>
        <main>
          <MonitorCard
            success={this.state.success}
            // new 키워드를 통해 인스턴스 생성을 하지 않지만,
            // 내부적으로는 분명히 인스턴스이므로 this라는 키워드를 사용
            failure={this.state.failure}
          />
          <PlayButton
            monitoring={false}
            onPlay={this.onStart}
            onPause={this.onStop}
          />
        </main>
      </div>
    );
  }
}

```

## 리액트의 특징

- class를 만들지만 다른 곳에서 인스턴스로 만들어 사용하지 않음
- 리액트가 내부적으로 인스턴스를 만드는 작업을 함. 그래서 사용자가 `new` 연산자를 사용하여 인스턴스를 만들지 않을 뿐
- 타입스크립트는 자바스크립트가 OOP의 특징을 갖도록 도움을 주는데, 그런 점에서 리액트는 타입스크립트의 일부만 사용하는 거라 볼 수 있다.



# 정리하며

정리하는 게 너무... 귀찮기도 하고 할 일이 많은 터라 쉽지 않았는데 정리하기 잘한 것 같다. (*늘 투덜거리면서 정리 안하면 불안해한다*) 정리하면서 어제 배운 걸 한 번 더 복습할 수 있어서 좋았고, 정리하면서 완벽히(*과연?*) 이해했는데 시간 지나면 또 까먹겠지? 미래의 나 과거의 나를 칭찬해주길...