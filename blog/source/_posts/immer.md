---
title: immer
date: 2019-05-31 12:31:18
tags:
categories:
- 개발공부
- javascript
---

# immer

- 자바스크립트의 객체 불변성을 유지하기 위해 사용하는 모듈 중 하나
- 같은 기능을 하는 모듈,  `Immutable`가 유명하다.

## 사용해보자

일단, 패키지를 깔아준다. `yarn add immer`

그리고 사용할 곳에 임포트 한다. `import produce from immer`

### produce

상태와 같은 객체를 인자로 받으며, 인자로 들어오는 객체는 불변성 관리를 하지 않아도 된다. 통상 인자로 들어오는 객체를 `draft`라 표기한다.

리액트에서 쓴다면 이렇게 쓸 수 있다.

```javascript
this.setState(
  produce(draft => {
    draft.input = e.target.value;
  })
);

this.setState(
  produce(draft => {
    draft.todos.push({
      id: ++this.id,
      text: this.state.input,
      done: false
    });
    draft.input = "";
  })
);
```

