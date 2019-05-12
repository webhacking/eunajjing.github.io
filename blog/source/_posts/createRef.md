---
title: React.createRef()
date: 2019-05-07 11:29:18
tags:
categories:
- 개발공부
- React
---

# `React.createRef()`

> ## Ref
>
> - DOM에 접근해 직접 다루어야 할 때 사용한다.
>
> - 컴포넌트 내부에서만 작동하기 때문에 전역적이지 않다

기존의 `ref`생성 신텍스는

```javascript
<input ref={(ref) => {this.refname = ref}} />
// 이제 이 input 요소를 제어할 때는
// this.refname.블라블라();
// 형식으로 제어한다.
```

**그런데** `React.createRef()`을 사용할 경우

```javascript
class ReactTest extends Component {
	refname = React.createRef();
	// 위에서 선언해주고
	
	render() {
		<input ref={this.refname}/>
		// 이렇게 쓴다!
	}
	
	만약 포커싱 같은 것을 주고 싶으면
	this.refname.currnet.focus();
} 
```

**currnet가 핵심**

