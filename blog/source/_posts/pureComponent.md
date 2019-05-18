---
title: PureComponent
date: 2019-05-18 15:30:18
tags:
categories:
- 개발공부
- React
---

# PureComponent

리액트에는 컴포넌트를 만드는 방법이 크게 3가지이다.

- 일반 컴포넌트
- 퓨어 컴포넌트
- 함수형 컴포넌트

이 중 일반 컴포넌트와 퓨어 컴포넌트에 대해 알아보고자 한다.

## 큰 차이는, `shouldComponentUpdate()`

퓨어 컴포넌트는 `shouldComponentUpdate()`가 이미 구현되어 있으며, `state`, `props`를 얕은 비교를 하여 변경된 것이 있을 때 컴포넌트를 리렌더링한다.

> ### 얕은 복사, 깊은 복사
>
> #### 얕은 복사
>
> ```javascript
> const a = {1, 2, 3, 4, 5};
> const b = a;
> ```
>
> #### 깊은 복사
>
> 자바스크립트는 기본 자료형(`number`, `String`, `boolean`)은 값을 복사할 때 완전히 복사한다.
>
> ```javascript
> const a = 1;
> const b = a;
> ```
>
> 이 경우 `a`와 `b`는 독립적이다.
>
> **그럼 객체의 경우에는**
>
> ```javascript
> const a = {1, 2, 3, 4, 5};
> const b = {...a};
> // 객체 for문을 돌려서 하나하나 때려박던가 assign()을 이용하던가
> ```

때문에 만약 `state`나 `props`가 복잡한 구조를 가지고 있다면 얕은 비교로는 리렌더링 여부를 결정할 수 없기 때문에 제어가 힘들다.

아, 퓨어 컴포넌트 안에서도 `shouldComponentUpdate()`를 기재해서 사용할 수 있다.

```javascript
import React, { PureComponent } from 'react';

class App extends PureComponent {
	(...)
}
```

