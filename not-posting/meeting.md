---

---

# 제목을 뭐라고 붙이지

## 브라우저의 동작 원리

### 브라우저의 구조

- 사용자 인터페이스
  - 주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등. 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분

- 브라우저 엔진
  - 사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어

- **렌더링 엔진**
  - 요청한 콘텐츠를 표시

- 통신
  - HTTP 요청과 같은 네트워크 호출에 사용

- UI 백엔드
  - 기본적인 장치를 그림
  - 플랫폼에서 명시하지 않은 일반적인 인터페이스로 OS 사용자 인터페이스 체계를 사용

- 자바스크립트 해석기
  - 자바스크립트 코드를 해석하고 실행

- 자료 저장소
  - 자료를 저장하는 계층
  - 쿠키를 저장하는 것과 같이 모든 종류의 자원을 하드 디스크에 저장할 필요
  - HTML5 명세에는 브라우저가 지원하는 웹 데이터 베이스가 정의

![](https://d2.naver.com/content/images/2015/06/helloworld-59361-1.png)

### 렌더링 엔진에서는 무슨 일이 벌어지나

![img](https://post-phinf.pstatic.net/MjAxNzA3MDJfMTEy/MDAxNDk4OTI1MjU4OTY3.OR6bSfoKWEl_T4gQ1RDjKZ7dimgljLViK9QRPe9uQRQg.3FOoFdCJQGOBFEErH1EXUtHKnUfTg0-kkmnToN3-LHsg.PNG/image_3480670191498924844279.png?type=w1200)

1. 서버에서 응답으로 받은 HTML 데이터를 파싱한다.

2. HTML을 파싱한 결과로 DOM Tree를 만든다(빌드한다). (**"무엇을"** 그릴지 결정한다.)

   - 바이트 → 문자 → 토큰 → 노드 → 객체 모델

     ![](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/full-process.png?hl=ko)

     - 변환
       - 브라우저가 HTML의 원시 바이트를 디스크나 네트워크에서 읽어와서, 해당 파일에 대해 지정된 인코딩(예: UTF-8)에 따라 개별 문자로 변환한다
     - 토큰화
       - 브라우저가 문자열을 W3C HTML5 표준에 지정된 고유 토큰으로 변환
     - 렉싱
       - 방출된 토큰은 해당 속성 및 규칙을 정의하는 객체로 변환
     - DOM 생성
       - 생성된 객체는 트리 데이터 구조 내에 연결

3. 파싱하는 중 CSS 파일 링크를 만나면 CSS 파일을 요청해서 받아온다.

4. CSS 파일을 읽어서 CSSOM(CSS Object Model)을 만든다(빌드한다). (**"어떻게"** 그릴지 결정한다.)

5. DOM Tree와 CSSOM이 모두 만들어지면 이 둘을 사용해 Render Tree를 만든다. (**"화면에 그려질 것만"** 결정)

6. Render Tree에 있는 각각의 노드들이 화면의 어디에 어떻게 위치할지 계산하는 Layout과정을 거쳐서  (**"Box-Model"**  생성)

7. 화면에 실제 픽셀을 Paint한다.

> 렌더링 엔진은 좀 더 나은 사용자 경험을 위해 가능하면 빠르게 내용을 표시하는데 모든 HTML을 파싱할 때까지 기다리지 않고 배치와 그리기 과정을 시작한다. 네트워크로부터 나머지 내용이 전송되기를 기다리는 동시에 받은 내용의 일부를 먼저 화면에 표시한다.

> 출처
>
> - [브라우저는 어떻게 동작하는가](https://d2.naver.com/helloworld/59361)
> - [브라우저의 동작 원리]([https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/FrontEnd#%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/FrontEnd#브라우저의-동작-원리))
> - [**브라우저는 웹페이지를 어떻게 그리나요?**](https://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)

## HTTP 접근 제어, CORS

**웹 서버에게 보안 cross-domain 데이터 전송을 활성화하는 cross-domain 접근 제어권을 부여하는 것**. 허가된 **출처 집합**을 서버에게 알려주도록 허용하는 특정 HTTP 헤더를 추가함으로써 동작한다.

- 등장 배경

  다른 도메인으로부터 리소스가 요청될 경우 해당 리소스는 **cross-origin HTTP 요청** 에 의해 요청된다.
  - 예를 들어 http://domain-a.com으로부터 전송되는 HTML 페이지가 `<img> src` 속성을 통해 http://domain-b.com/image.jpg를 요청하는 것

  하지만 대부분의 브라우저들은 보안 상의 이유로 스크립트에서의 cross-origin HTTP 요청을 제한한다. 이것을 `Same-Origin-Policy(동일 근원 정책)`이라고 한다. 요청을 보내기 위해서는 요청을 보내고자 하는 대상과 프로토콜, 포트도 같아야 함을 의미한다.

### Preflight Request

- 실제 요청 전에 인증 헤더를 전송하여 서버의 허용 여부를 미리 체크하는 테스트 요청

- HTTP 의 `OPTIONS` 메서드를 사용하며 `Access-Control-Request-*` 형태의 헤더로 전송

> 출처
>
> - [HTTP 접근 제어](https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS)
> - [CORS](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/FrontEnd#cors)

## 클라이언트 사이드 렌더링

### 클라이언트 사이드 렌더링

- 서버는 단순히 데이터를 보내주는 역할만을 수행
- HTML을 그리는 역할은 클라이언트의 자바스크립트가 수행
- 클라이언트 단에서 렌더링을 수행하기 때문에 비교적 일관성 있는 코드 작성 가능

#### 장점

- 사용자에게 필요한 것만 읽어들이기 때문에 섭사렌보다는 빠른 인터렙션 기대 가능

#### 단점

- 초기 구동 속도가 느림
- SEO 문제
- 보안 문제 : 쿠키 외에는 사용자 정보를 저장할 공간이 부재

> 출처
>
> - [서버사이드 렌더링과 클라이언트 사이드 렌더링](https://asfirstalways.tistory.com/244)

## CSS 방법론

### SMACSS

- 스타일을 다섯 가지 유형으로 분류하고, 각 유형에 맞는 선택자(selector)와 작명법(naming convention)을 제시
  - 기초
  - 레이아웃
  - 모듈
  - 상태
  - 테마
- 핵심은 **범주화**

### OOCSS

- 객체지향 CSS 방법론
  - 구조와 모양을 분리한다.
  - 컨테이너와 컨텐츠를 분리한다.

### BEM

- BEM 의 B 는 “Block”이다.
  - 블록(block)이란 재사용 할 수 있는 독립적인 페이지 구성 요소를 말하며, HTML 에서 블록은 class 로 표시된다. 블록은 주변 환경에 영향을 받지 않아야 하며, 여백이나 위치를 설정하면 안된다.
- BEM 의 E 는 “Element”이다.
  - 블록 안에서 특정 기능을 담당하는 부분으로 block_element 형태로 사용한다. 요소는 중첩해서 작성될 수 있다.
- BEM 의 M 는 “Modifier”이다.
  - 블록이나 요소의 모양, 상태를 정의한다. `block_element-modifier`, `block—modifier` 형태로 사용한다. 수식어에는 불리언 타입과 키-값 타입이 있다.

> 출처
>
> - [[CSS방법론] SMACSS, BEM, OOCSS](https://wit.nts-corp.com/2015/04/16/3538)
> - [CSS Methodology](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/FrontEnd#css-methodology)

## normalize vs reset

### reset

기본적으로 제공되는 브라우저 스타일 전부를 **제거**하기 위해 사용된다.

### normalize 

브라우저 간 일관된 스타일링을 목표로 한다. 요소는 브라우저간에 일관된 방식으로 굵게 표시됩니다. 추가적인 디자인에 필요한 style 만 CSS 로 작성해주면 된다.

## javascript의 구조

### call Stack

- 요청이 들어올 때마다 call Stack에 새로운 프레임이 생기고, push되고 실행이 끝나면 pop된다.

### Heap

- 동적으로 생성된 객체가 할당된다.

### Task Queue

- 처리해야하는 Task들을 보관하는 임시 대기 큐가 있고, call Stack이 비워지면 대기열에 들어온 순서대로 실행된다
- 비동기로 호출되는 함수들은 call Stack이 아니라 task Queue에 할당된다.

> 출처
>
> - [자바스크립트의 이벤트 루프](https://asfirstalways.tistory.com/362)

## 클로저

```javascript
function makeAdder(x) {
  var y = 1;
  return function(z) {
    y = 100;
    return x + y + z;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

함수 + 함수를 둘러싼 환경을 기억하는 함수, 즉 `function(z) {
    y = 100;
    return x + y + z;
  };`, `add(5)`, `add10()`이 클로저

> 출처
>
> - [**자바스크립트의 스코프와 클로저**](https://meetup.toast.com/posts/86)
> - [클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)
> - [JavaScript 클로저(Closure)](https://hyunseob.github.io/2016/08/30/javascript-closure/)

## this

- 기본적으로 전역 객체(브라우저 상에서는 `window`)
  - 함수 안에서 사용하지 않을 경우 기본적으로 전역 객체다
- 객체의 프로퍼티가 함수일 때(메서드), 해당 함수 안에서의 `this`는 객체이다
- 객체의 프로퍼티가 함수일 때(메서드), 메서드 안 함수 안의 `this`는 전역 객체이다
- 생성자 함수를 통해 생성한 객체의 경우 `this`는 자신이다

### `apply()`, `call()`

`this`를 넘길 때 쓰는 함수, 특정한 객체에 `this`를 묶을 수 있다

```javascript
// call()이나 apply()의 첫 번째 매개변수로 객체를 제공하면 this가 그 객체에 묶임
var obj = {a: 'Custom'};

// 전역 객체에 속성 지정
var a = 'Global';

function whatsThis() {
  return this.a;  // 함수 호출 방식에 따라 값이 달라짐
}

whatsThis();          // 'Global'
whatsThis.call(obj);  // 'Custom'
whatsThis.apply(obj); // 'Custom'
```

### `bind()`

```javascript
function f() {
  return this.a;
}

var g = f.bind({a: 'azerty'});
console.log(g()); // azerty

var h = g.bind({a: 'yoo'}); // bind는 한 번만 사용 가능!
console.log(h()); // azerty

var o = {a: 37, f: f, g: g, h: h};
console.log(o.a, o.f(), o.g(), o.h()); // 37,37, azerty, azerty
```

- `bind()`로 할당된 변수의 경우 같은 본문(코드)과 범위를 가졌지만 this는 원본 함수를 가진 새로운 함수를 생성한다.
- 새 함수의 `this`는 호출 방식과 상관없이 영구적으로`bind()`의 첫 번째 매개변수로 고정된다.

> 출처
>
> - [JavaScript this 정리](https://hyunseob.github.io/2016/03/10/javascript-this/)
> - [this](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)

## 함수형 프로그래밍

- 컨셉
  - 변경 가능한 상태를 불변하게 만들어 사이드이팩트를 없앤다
  - 모든 것은 객체
    - 함수가 1급 객체이다
      - 1급 객체의 조건
        - 변수나 데이터에 할당할 수 있다
        - 객체의 인자로 넘길 수 있다
        - 객체의 리턴 값으로 리턴할 수 있다
  - 코드를 간결하게 하고 가독성을 높여 구현할 로직에 집중한다
  - 동시성 작업을 보다 쉽고 안전하게 구현한다

### 순수함수

어떤 함수에 동일한 인자를 주었을 때 항상 같은 값을 리턴하는 함수

```javascript
const testFunc = (a, b) => a+b;
```

이것은 순수 함수이지만,

```javascript
let testNum = 10;
const testFunc2 = (a, b) => a+b+c;
```

이것은 순수 함수가 아니다 (`testNum`의 값이 바뀌면 항상 같은 값을 리턴하지 않기 때문)

### OOP와 함수형 프로그래밍의 차이점

- 1급 객체가 다르다
  - OOP의 경우 1급 객체가 클래스인 반면, 함수형 프로그래밍은 함수이다
- 객체지향은 프로그램을 상호작용하는 객체들의 집합으로, 함수형은 상태 값을 지니지 않는 함수 값들의 연속으로 생각

> 출처
>
> - [함수형 프로그래밍이란]([https://medium.com/@lazysoul/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80-d881230f2a5e](https://medium.com/@lazysoul/함수형-프로그래밍이란-d881230f2a5e))
> - [순수 함수란](https://jeong-pro.tistory.com/23)
> - [함수형 프로그래밍과 객체지향 프로그래밍](https://madplay.github.io/post/functional-programming-object-oriented-programming)