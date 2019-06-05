---
title: 우아한테크러닝 Typescript & React 101 06 UI with hook
date: 2019-06-05 14:42:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
---

# hook을 사용한 UI 상태 제어

## `setState` 의 역할

- 상태를 바꾼다
- 상태를 받고 있는 컴포넌트를 재렌더링한다
  - 예제에서 사용한 훅은 컴포넌트 재렌더링을 위한 것

## 예제 코드

```javascript
import * as React from "react";
import { Row, Col, Button, Icon, Input, PageHeader as Header } from "antd";
import { IAuthentication } from "../store";
import { Maybe } from "./Maybe";

const DEFAULT_PICTURE = "https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png";

interface IProps {
  picture?: string;
  label?: string;
  authentication?: IAuthentication;
  requestLogout?: () => void;
  openNotificationCenter?: () => void;
}

export const PageHeader: React.FC<IProps> = ({label}) => {
  
  const [activeSearch, toggleSearch] = React.useState(false);
  // hook 사용
  // 해당 상태의 역할은 toggleSearch가 호출되면 Ui가 다시 만들어지도록 하는 것

  return (
    <Row>
      <Col>
        <Header title={<span>{label}</span>}
          extra={[
            <Maybe key="1" test={activeSearch}>
            // 3항 연산자 컴포넌트 사용
            // 훅으로 만든 상태가 true이면 아래의 컴포넌트가 렌더된다
              <Input.Search onBlur={() => toggleSearch(false)} />
						// 포커스가 해당 인풋에 사라지면 setter가 호출되며 해당 컴포넌트가 새로 렌더링될 것
            </Maybe>,
            <Maybe key="2" test={!activeSearch}>
            // 훅으로 만든 상태가 false면 아래의 컴포넌트가 렌더된다
              <Button onClick={() => toggleSearch(true)}
                shape="circle" icon="search" />
            </Maybe>
						// 중간 요소들 생략
          ]}
        />
      </Col>
    </Row>
  );
};
```

