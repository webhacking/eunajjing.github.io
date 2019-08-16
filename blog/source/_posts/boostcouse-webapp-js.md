---
title: 웹앱 개발 - javascript, DOM
date: 2019-07-25 15:39:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# javascript

## 배열

- 배열 안에는 **모든 타입이 다 들어갈 수 있음**
  - 함수도 배열 안에 배열도, 배열 안에 객체도 들어갈 수 있음
- 자바스크립트 배열은 기본적인 `stack` 구조를 지닌다
  - `arr.push(blah)`, `arr.pop()`을 사용할 수 있다

### 유용한 메서드

- `arr.indexOf()`
  - 호출한 `String` 객체에서 주어진 값과 일치하는 첫 번째 인덱스 반환
  - 모두 일치하지 않으면 `-1` 반환
- `arr.join()`
  - 배열의 요소들을 문자열로 합칠 수 있다
- `arr.concat()`
  - 배열을 복사
- `arr.forEach(item => item)`
- `arr.map((item, index) => {})`
- `arr.filter(num => num > 0)`

## 객체

`key`, `value` 구조의 자료구조

> **객체 속성의 삭제**
>
> ```javascript
> delete myFriend.key;
> delete myFriend["key2"];
> ```

### 객체의 탐색

- `for(객체 in item)`
- `Object.keys(객체).forEach(key => key)`

> [실습 예제](https://gist.github.com/nigayo/a9a118977f82780441db664d6785efe3)
>
> - type이 sk인, name으로 구성된 배열만 출력
>
> ```javascript
> [{
> 	"id": 1,
> 	"name": "Yong",
> 	"phone": "010-0000-0000",
> 	"type": "sk",
> 	"childnode": [{
> 		"id": 11,
> 		"name": "echo",
> 		"phone": "010-0000-1111",
> 		"type": "kt",
> 		"childnode": [{
> 				"id": 115,
> 				"name": "hary",
> 				"phone": "211-1111-0000",
> 				"type": "sk",
> 				"childnode": [{
> 					"id": 1159,
> 					"name": "pobi",
> 					"phone": "010-444-000",
> 					"type": "kt",
> 					"childnode": [{
> 							"id": 11592,
> 							"name": "cherry",
> 							"phone": "111-222-0000",
> 							"type": "lg",
> 							"childnode": []
> 						},
> 						{
> 							"id": 11595,
> 							"name": "solvin",
> 							"phone": "010-000-3333",
> 							"type": "sk",
> 							"childnode": []
> 						}
> 					]
> 				}]
> 			},
> 			{
> 				"id": 116,
> 				"name": "kim",
> 				"phone": "444-111-0200",
> 				"type": "kt",
> 				"childnode": [{
> 					"id": 1168,
> 					"name": "hani",
> 					"phone": "010-222-0000",
> 					"type": "sk",
> 					"childnode": [{
> 						"id": 11689,
> 						"name": "ho",
> 						"phone": "010-000-0000",
> 						"type": "kt",
> 						"childnode": [{
> 								"id": 116890,
> 								"name": "wonsuk",
> 								"phone": "010-000-0000",
> 								"type": "kt",
> 								"childnode": []
> 							},
> 							{
> 								"id": 1168901,
> 								"name": "chulsu",
> 								"phone": "010-0000-0000",
> 								"type": "sk",
> 								"childnode": []
> 							}
> 						]
> 					}]
> 				}]
> 			},
> 			{
> 				"id": 117,
> 				"name": "hong",
> 				"phone": "010-0000-0000",
> 				"type": "lg",
> 				"childnode": []
> 			}
> 		]
> 	}]
> }]
> ```
>
> **답안**
>
> ```javascript
> let answer  = []
> function names_in_sk(datas) {
>     // function 키워드 삭제 시 안됨
>   datas.forEach(d => {
>       if (d.type == "sk") answer.push(d.name)
>       if(d.childnode)  names_in_sk(d.childnode)
>   })
> }
> ```

# DOM

> `DOM` 노드도 1부터 시작된다

## DOM 엘리먼트 오브젝트

- `tagName` : 엘리먼트의 태그명 반환

- `textContent` : 노드의 텍스트 내용을 설정하거나 반환

  ```javascript
  const el = document.querySelector("#blahblah");
  //el은 객체임
  
  el.tagName
  //해당 노드의 태그명이 나옴
  
  el.textContent
  //해당 노드가 가지고 있는 텍스트 노드들이 출력됨
  ```

- `nodeType` : 노드의 노드 유형을 숫자로 반환

### 탐색

- `childNodes`
  - 엘리먼트의 자식 노드의 노드 리스트 반환(텍스트 노드, 주석 노드 포함)
- `firstChild`
  - 엘리먼트의 첫 번째 자식 노드를 반환
  - **공백이나 문자열도 첫 자식 요소로 인정**
- `firstElementChild`
  - 첫 번째 **자식 엘리먼트**를 반환
- `parentElement`
  - 엘리먼트의 부모 엘리먼트 반환 
- `nextSibling`
  - 동일한 노드 트리 레벨에서 다음 노드를 반환 
- `nextElementSibling`
  - 동일한 노드 트리 레벨에서 다음 엘리먼트 반환

### 조작

> 개발자 도구에서 특정 엘리먼트를 클릭하면 콘솔에서 `$0`으로 접근 가능

- `removeChild()`

  - 엘리먼트의 자식 노드 제거 

- `appendChild()`

  - 마지막 자식 노드로 자식 노드를 엘리먼트에 추가

  ```javascript
  const div = document.createElement("div")
  
  const text = document.createTextNode("blahblah")
  
  div.appendChild(text)
  
  $0.appendChild(div)
  ```

- `insertBefore()`

  - 기존 자식노드 앞에 새 자식 노드를 추가

  ```javascript 
  const div = document.createElement("div")
  
  const text = document.createTextNode("blahblah")
  
  div.appendChild(text)
  
  const parent = document.querySelector(".tableClass tbody")
  
  const front = document.querySelector(".tableClass tr:nth-child(3)")
  
  parent.insertBefore(div, front)
  // front 앞에 div를 추가
  ```

- `cloneNode()`

  - 노드를 복제

- `replaceChild()`

  - 엘리먼트의 자식 노드 바꿈

- `closest()`

  - 상위로 올라가면서 가장 가까운  엘리먼트를 반환

- `remove()`

  - 해당 노드의 엘리먼트를 삭제

### HTML 문자열 처리

- `innerText`

  - 지정된 노드와 **모든 자손의 텍스트** 내용을 설정하거나 반환

  ```javascript
  $0.innerText += "추가할 문자열"
  ```

- `innerHTML`

  -  지정된 엘리먼트의 내부 html을 설정하거나 반환

  ```javascript
  const change = el.innerHTML
  // 해당 노드의 자손 요소들이 전부 문자열로 출력됨
  
  change.innnerHTML = "<p>blahblah</p>"
  // 해당 노드의 자손 요소들에 덮어씌워짐 
  ```

- `insertAdjacentHTML()`

  - HTML로 텍스트를 지정된 위치에 삽입
  - `beforebegin`
  - `afterbegin`
  - `beforeend`
  - `afterend`

  ```javascript
  const div = document.querySelecotr("div")
  
  div.insertAdjacentHTML("afterbegin", "<p>blahblah</p>")
  // div 태그 바로 다음에 p 태그를 넣는다
  
  div.insertAdjacentHTML("beforebegin", "<p>blahblah</p>")
  // div 태그 위에 p 태그를 붙인다
  ```

## 영상에서 다루지 않은 것

- `tag:nth-of-type(n)` : 특정 태그의 `n` 번째 자식 요소를 찾는다
- `tag:nth-child(n)` : `dom` 요소 중에서 `n` 번째 자식 요소를 찾되, **해당 태그가 제대로 명기되어야 해당 노드의 객체**를 가져온다
  - 만약 해당 태그가 제대로 명기되지 않았다면 `undefined`가 뜬다