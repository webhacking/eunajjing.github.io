---
title: javascript 중괄호와 소괄호의 늪
date: 2019-06-14 17:23:18
tags:
categories:
- 개발공부
- javascript
---

# 중괄호와 소괄호의 늪

계속 반복되는 실수가 나와서, 또 미래의 내가 헤멜 가능성이 농후해서 적는다. 객체 리턴할 적에 하던 대로 중괄호로 시작하면 에러 난다. 즉

```javascript
export const blahblah = (a = {}) => {{
  ...a,
  blahblah : ""
}};
```

위의 코드는 에러가 나기에, 아래처럼 써줘야 한다.

```javascript
export const blahblah = (a = {}) => ({
  ...a,
  blahblah : ""
});
```

