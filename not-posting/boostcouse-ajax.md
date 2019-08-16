---
title: AJAX
date: 2019-07-16 15:39:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# AJAX

```javascript
const ajax => ()  => {
	const oReq = new XMLHttpRequest();

  oReq.addEventListener("load", () => {
    console.log(this.responseText);
  });

  oReq.open("GET", "http://www.example.org/example.txt");
  oReq.send();
}
```

![](https://cphinf.pstatic.net/mooc/20180202_278/15175639688702H54K_PNG/3-3-1_Ajax%28Jquery__%29.png)

```
var json객체로변환된값 = JSON.parse("서버에서 받은 JSON 문자열");
```

