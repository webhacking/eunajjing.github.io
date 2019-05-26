---
title: 렌더링의 최소화
date: 2019-05-27 00:13:18
tags:
categories:
- 개발공부
- React
---

# 렌더링의 최소화

## `shouldComponentUpdate`의 이용

라이프 메서드 중에서 `shouldComponentUpdate`의 인자 값, `nextProps`와 `nextState`를 사용하여 렌더링 여부를 결정한다.

```javascript
shouldComponentUpdate(nextProps, nextState) {
    // 바뀐 프롭스로 nextProps가 들어오니까
    return nextProps.todo !== this.props.todo;
  	// 해당 컴포넌트가 바뀐 프롭스와 일치하는지를 검사
  	// 만약 일치한다면 false를(일치하므로 또 렌더링할 필요 없음),
  	// 일치하지 않는다면 true를 반환하는 로직
  }
```

## `react-virtualized`의 이용

```javascript
import React, { Component } from "react";
import TodoItem from "./TodoItem";
import { List } from "react-virtualized";

class TodoList extends Component {
  renderRow = ({ index, key, style, parent }) => {
    const { todos, onToggle, onRemove } = this.props;
    const todo = todos[index];
    return (
      <TodoItem
        id={todo.id}
        checked={todo.checked}
        text={todo.text}
        todo={todo}
        onToggle={onToggle}
        onRemove={onRemove}
        key={key}
        style={style}
      />
    );
  };

  render() {
    const { todos } = this.props;

    return (
      <div className="TodoList">
        <List
          width={600}
          height={364}
          rowCount={todos.length}
          rowHeight={62}
          rowRenderer={this.renderRow}
          list={todos}
          style={{ outline: "none" }}
        />
      </div>
    );
  }
}
```

이 경우 보여지는 영역에만 있는 `TodoItem`만 렌더링되고, 보여지지 않는 것들은 스크롤 이벤트가 발생해서 브라우저에 해당 `Item`이 등장했을 때 렌더링이 된다. 자세한 내용은 [공식 문서](https://github.com/bvaughn/react-virtualized) 참조