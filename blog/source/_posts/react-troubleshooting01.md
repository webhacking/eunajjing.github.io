---
title: 컴포넌트를 대문자로 쓰지 않았더니
date: 2019-05-16 13:23:18
tags:
categories:
- 개발공부
- React
---

# 직접 만든 컴포넌트는 반드시 대문자로

아무 생각 없이 소문자로 만들었더니 렌더링이 안됐다.

```javascript
const test = () => {
	return (
		<div>
			...
		</div>
	);
}

const parent = () => {
  return (
  	<div>
    	<test />
    </div>
  );
}
```

이러면 안됨

```javascript
const Test = () => {
	return (
		<div>
			...
		</div>
	);
}

const parent = () => {
  return (
  	<div>
    	<Test />
    </div>
  );
}
```

이렇게 하면 됨

