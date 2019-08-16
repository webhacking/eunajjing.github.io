---
title: google I/O extended Seoul 후기
date: 2019-07-02 23:03:18
tags:
categories:
- 일상
---

# GDG I/O Extended Seoul

GDG 상반기의 가장 큰 행사, I/O Extended에 다녀왔다. 금주에 너무 많은 일들이 있던 터라, 행사 당일에도 갈까말까를 엄청 고민했었는데. 결국 두 개의 세션만 듣고 귀가했다. ~~몸 상태가 너무 안 좋아서….~~ 그래도 늘 그렇듯 유익한 시간이었고, 향후 웹테크도 신청해뒀으니 그 때 더 배워야지, 하는 생각으로 돌아왔다.

## What's new in Web

### 웹 업데이트

#### 신속성

- 모던 웹의 문제점 : 로드할 콘텐츠가 너무 많아 초기 렌더링할 때 느림

  - 해결을 위해 `image-lazy-loading` 도입

    - 사용자가 볼 수 있는 영역만 로딩하는 식으로, `html` 단에서는 이렇게 기재하면 된다.

    ```html
    <img src="" loading="lazy" />
    ```

    - 해당 `img`가 클라이언트의 뷰에 등장하는 순간 로딩이 시작된다.

  - Portal

    - `iframe`이 최상위 레이어에 올라오지 못하는 반면, `portal`로 생성된 페이지의 경우 최상위 레이어로 올라오며, **해당 콘텐츠로 이동**이 가능하다.

      - 유저가 뉴스 사이트를 볼 때 기사 본문 아래 다른 관련 기사  링크를 포털로 구현한다면, 새 페이지로 끊김없이 이동할 수 있도록 하는 등이 가능하다.

        ![](https://cdn.clien.net/web/api/file/F01/8441837/141ad98d647f49.mp4)
  
  - lighthouse
  
    - `budget.json` 구현
    
      - 즉 로딩 속도 개선을 위해 어떤 항목의 로딩 속도에 예산을 잡는 행위
    
      - 그러나 예산을 잡는 게 처음하는 이에게는 약간 모호할 수 있어, 해당 `json`을 생성해주는 사이트도 존재
        - [`performance budget`를 만들어준다](http://www.performancebudget.io/)

#### 확장성

- [web perception toolkit](https://perceptiontoolkit.dev/)
  
  - Sensing : 인식
  - Meaning : 인식한 것을 웹 페이지로 전송
  - Rendering : 사용자에게 결과를 보여줌
  
- [Web Share Target](https://developers.google.com/web/updates/2018/12/web-share-target)
  
  - 네이티브 앱에서는 가능하던 기능이 구현됨
  - url을 사용하지 않고 공유가 가능해짐
  
- duplex on the web
  - 구글 어시스턴트를 사용할 수 있다
    - 이미 알고 있는 정보를 구글 어시스턴트가 입력 폼에 자동으로 넣어주는 기능 등
  
    > 키노트 영상에서 25:00에서 확인 가능하다

#### 안전성

- `https`를 통해 사용자 보호

- 프라이버시 제어 단을 유저가 더 쉽게 접근할 수 있도록 ui 개선

- same site cookie

  - 쿠키를 생성한 도메인과 같은 출처일 때만 쿠키를 전송

  - 헤더에 해당 속성을 추가

    ```
    Set-Cookie: SameSite=Strict
    ```
    
  - 만약 속성을 추가하지 않는다면, 기본 값은 `Lax`

    - 비교적 안전한 cross-site 요청만 허용

- Fingerprinting protection

  - 불필요한 생체 인식을 최소화 시키도록 노력

### 웹의 한계를 극복하기 위한 시도

`app gap` : 네이티브 앱에서는 가능하지만 웹에서는 불가능한 것들

- 사용자의 파일 시스템에 접근, 전화번호에 접근 등
  - 이런 한계들을 극복하기 위한 시도
    - [Badging API](https://developers.google.com/web/updates/2018/12/badging-api)
    - [Get Installed Related Apps API](https://developers.google.com/web/updates/2018/12/get-installed-related-apps)
    - [Shape Detection API](https://developers.google.com/web/updates/2019/01/shape-detection)
    - [Wake Lock API](https://developers.google.com/web/updates/2018/12/wakelock)
    - [Writable Files API](https://developers.google.com/web/updates/2018/11/writable-files)
- 웹은 인터넷이 불안정하거나 연결되지 않은 경우 아무 것도 할 수 없음 
  - pwa

> proj-fupu : 사용자에게 해를 끼치면서까지 기능을 구현하지 않는다

## 구글 검색과 자바스크립트 사이트

> - [발표 자료](https://drive.google.com/file/d/1cArGPApYvbrAVP01X7-Q1BVsRqHyKVDE/view)
> - [연사자 분 관련 블로그 포스팅](https://medium.com/@euncho/google-검색은-어떻게-동작하는가-fbc0e421a97a)

### 구글은 어떻게 사이트를 크롤링해가나

#### googlebot

구글에서 웹 페이지를 수집하기 위해 사용하는 크롤러. 전통적인 웹 사이트에서는 동작에 아무런 무리가 없었지만, 현대의 웹 사이트는 `body` 영역이 비어있는 **js가 콘텐츠를 생성**한다. `googlebot`은 js 실행을 하지 못하므로,

#### Puppeteer

Headless Chrome을 더 잘 구동시키기 위한 Node API

> **Headless**
>
> : 브라우저 창을 실제로 운영체제의 '창'으로 띄우지 않고 대신 화면을 그려주는 작업(렌더링)을 가상으로 진행해주는 방법

#### 현재 google의 검색은

Puppeteer를 이용해 페이지를 렌더하고 렌더된 결과물을 googlebot을 이용해 수집. **즉, SPA가 SEO에 영향을 주지 않는다.**

### 구글 상위 검색에 노출되기

#### 올바른 `<title>` 값 사용

```html
<title>콘텐츠 제목 - 사이트 제목</title>
```

#### 올바른 `discription` 사용

```html
<meta name="discription" content="">
```

#### 올바른 `html` 사용

##### Navigation

구글봇은 기본적으로 `a link`를 따라 움직이기 때문에, 해당 페이지가 검색에 잘 걸려야 한다면 `a link` 를 적극 활용한다.

###### 좋은 예

```html
<a href=“page”>페이지 이동</a>
<a href=“page” onClick=“beforeMove()”>페이지 이동</a>
```

###### 나쁜 예

```html
<button onClick=“move(‘page’)”>페이지 이동</button>
<div onClick=“move(‘page’)”>페이지 이동</a>
```

##### 제목

`h` 태그를 적극 활용하여 구글봇에게 어떤 항목이 중요한 것인지 인지시킨다.

`div` 등을 활용한 태그는 구글봇이 알 수 없다.

##### 반응형 웹

구글 봇은 반응형 웹을 지원하는 사이트를 우선적으로 노출시킨다.

##### Structured Data

해당 자료 항목은 [여기](https://schema.org/docs/gs.html#schemaorg)서 확인 가능

현재 구글이 지원하는 항목은 [여기](https://developers.google.com/search/docs/data-types/article)서 확인 가능 

###### 예시

```html
<div class="recipe" itemscope itemtype="http://schema.org/Recipe">
	<h1 class="recipe-title" itemprop="name">Chicken</h1>
	<p><meta itemprop="cookTime" content="PT40M">40분정도 소요됩니다</p>
	<ol class="list" itemprop="recipeInstructions">
		<li class="item">근처 시장이나 마트를 방문합니다</li>
		<li class="item">생닭을 구매합니다</li>
		<li class="item">냉동실에 넣습니다</li>
		<li class="item">배달 앱을 실행합니다</li>
		<li class="item">교촌치킨을 들어갑니다</li>
		<li class="item">허니 콤보에 레드소스를 추가합니다</li>
		<li class="item">결제합니다</li>
		<li class="item">당신은 맛잘알</li>
	</ol>
</div>
```

##### 속도

빠를 수록 더 상위에 노출된다.

> **Lighthouse**을 활용하여 웹페이지를 검사해보는 것도 좋은 방법
>
> 사이트가 구글봇을 기다리지 않고도 빨리 노출되길 바란다면, 구글의 [search console](https://search.google.com/search-console/welcome?utm_source=about-page)에 사이트를 등록하면 보다 빨리 된다.

### 구글 검색에 노출되지 않기

#### `Robots.txt`

Googlebot의 웹 콘텐츠 접근 권한을 제어하는 파일인 `Robots.txt`를 활용하여야 한다.

- 검색이 되면 안되는 도메인을 아예 분리하여 해당 도메인 자체를 검색 비활성화
- 인증 활용

> # SPA가 SEO 최적화와 무관하다면
>
> 구글 검색과 자바스크립트 사이트 세션을 듣고 궁금한 게 생겼다. **SPA가 SEO 최적화와 무관하다면** 도대체 SPA로 만들어진 사이트의 SEO를 돕는다는 `next.js`, `nuxt.js` 등은 왜 사용되는 것일까. 이 궁금증을 해결하기 위해 많은 문서를 뒤져봤지만, 대부분 SEO를 돕는다는 장점 외에는 달리 말이 없는 것 같아서 개인 SNS에 해당 궁금증을 업로드했다. 좋은 SNS 지인 분들이 공유를 해주셨고 며칠 지나 어떤 분께서 해답을 주셨다

### SSR 라이브러리의 필요성

- 구글봇이 SPA를 자동으로 크롤링하기 시작한 게 얼마 안되었다.
  - 우리가 대부분 알고 있는 SSR 라이브러리들은, 구글봇이 SPA 크롤링을 지원하기 전에 등장하였다.
- 다른 검색 포털 사이트의 크롤러들이 지원하지 않는 경우가 다수다.
- 오픈그래프 진영이 SPA를 완벽히 지원하지 못하고 있다.
  - 오픈그래프 사용을 위해서는 아래의 방법들 뿐이다
    - SSR 라이브러리
    - 프리렌더
- 구글이 SPA를 위한 비동기 스크립트를 기다린다고 해도, 완벽하지 않다.
  - SPA의 온전한 정보를 가져가고 있는지 알 수 없으며,
  - 크롤러가 어느 시점의 스냅샷을 완전히 로드했다고 판단하고 가져가는지 알 수 없다.
    - 이 부분을 확인하기 위해서는 어느 시점의 스냅샷이 완전한지를 `fetch as google` 같은 도구로 알려줄 수 있다.