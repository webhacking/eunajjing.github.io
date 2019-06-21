---
title: PWA 코드랩에 다녀왔다
date: 2019-06-12 00:17:18
tags:
categories:
- 개발공부
---

# [PWA 코드랩](https://festa.io/events/287) 참여

우연히 SNS 팔로잉하는 분이 코드랩을 진행하신다는 정보를 접해서, 이번 기회에 PWA가 무엇인지 배워보고자 참여하게 되었다. 리액트를 공부하면서 CRA로 만든 프로젝트의 경우 자동으로 서비스워커가 등록된 것들을 자주 보았는데, 도대체 얘가 뭐하는 앤지 답답하던 와중에 이번 코드랩으로 개념 정도는 알 수 있을 것 같아 신청했다.

> 코드랩 진행하시는 분은 이전에 [나의 개발 이야기](https://eunajjing.github.io/2019/04/29/for-developer/) 때 뵌 분이라 홀로 내적친밀감 느끼며 반가웠다. 그리고 바로 지난 날 같은 장소에서 우아한테크코스가 끝난 터라 ~~이건 또 후기 언제 쓰냐~~ 자정까지 달리는 비어 파티를 했는데, 12시간도 안되어서 그 장소에 있는 내가 지박령 같고 웃겼다. 더 웃겼던 건 나 같은 사람 여러 명이었어... 숙취에 쩐 우형 내부개발자들과 함께 힘들어하는 시간이었다...

## 코드랩 전에

SNS를 팔로잉해둔 덕에 올려주신 포스트를 미리 읽고 갈 수 있었다.

- [코드랩 가이드라인](https://medium.com/@euncho/pwa-코드랩-가이드라인-597049b2df40)

- [PWA를 구성하는 기술들](https://medium.com/@euncho/pwa를-구성하는-기술들-a5be57df5575)

한 번 미리 읽고 가기도 했고, 설명을 워낙 잘해주셔서 설명과 포스팅을 함께 보니 이해가 쉬웠다. ~~뭐야 PWA 울트라캡숑 멋지잖아~~ 이해했다고 해서 내가 당장 PWA를 구현할 수 있다는 뜻은 아니지만...

## 코드랩 진행

코드랩은 [이 예제](https://codelabs.developers.google.com/codelabs/your-first-pwapp/index.html?index=..%2F..index#0)를 각자 따라하는 식으로 진행되었다. 분명 30분이면 끝나는 예제인데 아무도 다 했다고 말하지 않아 ~~죄송해요 저도 다 했었는데 배고파서 도중에 나갔다왔고 추천해주신 예제 다 못했어요~~ 결국 제일 기본이 되는 예제를 함께 살펴보았다. 유익한 시간이었다.

## 그래서 난 뭘 배웠나

- PWA의 개념과 도입 이유
- 어설프게나마 구현하는 방식

### 느낀점

사실 코드랩 끝나고 난 직후에는 PWA가 엄청 좋게만 느껴졌는데, 현재 관련한 SNS 담론들을 팔로잉해보니

- 웹
- IOS
- 안드로이드

개발자가 거의 따로 있는 프로젝트의 경우(유저에게 보다 나은 경험을 주기 위해서는 따로 개발하는 것만큼 나은 게 없으니까) PWA까지 구현할 필요가 있냐는 이야기들이 나오고 있었고, 생각해봐야 하는 부분은 분명 있으리라 여겼다.

코드랩 진행 전에 PWA는 나온지 얼마 안 된 기술이고, 실제로 구현해본 사람도 별로 없다고 말씀하셨던 기억이 난다. 미래에 PWA가 어떻게 될지는 모르겠지만, 혼자 뭔가 만들어보는 걸 좋아하는 나로서는 웹을 디바이스에 깔 수 있다니 멋지게 느껴질 뿐이다. 토이프로젝트에 붙여봐야지.

# 예제

## `manifest.json`

### manifest란

앱에 대한 정보를 담은 JSON 파일

```json
{
  "name": "Weather", // 앱 이름
  "short_name": "Weather", // 풀네임이 나올 수 없을 때 띄워줄 앱의 짧은 이름
  "icons": [
    {
      "src": "/images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "start_url": "/index.html", // 시작할 url
  "display": "standalone", // 앱 실행 시의 속성
  // 이 외에 fullscreen, minimul-ui, browser 등이 있다.
  "background_color": "#3E4EB8", // 앱 실행될 때 배경색
  "theme_color": "#2F3BA2" // 상단바 색
}
```
이외에도 [다양한 옵션](https://developer.mozilla.org/en-US/docs/Web/Manifest#Members)들을 설정할 수 있다.

## `index.html`

```html
<link rel="manifest" href="/manifest.json" />
```

만들어놓은 `json` 파일을 불러올 수 있도록 설정한다. 해당 `json` 파일은 개발자 도구의 어플리케이션 메뉴에서 확인 가능하다.

![](https://codelabs.developers.google.com/codelabs/your-first-pwapp/img/c462743e1bc26958.png)

### 사파리를 위한 `meta` 태그 설정

사파리는 아직 manifest를 지원하지 않기 때문에 `manifest`에 쓴 설정들을 따로 기재해줘야 한다.

```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- 전체 화면 모드에서 실행 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- 상단바 색 적용, 색을 투명하게 할 때는 콘텐츠에 black-translucent를 적용 -->
<meta name="apple-mobile-web-app-title" content="Weather PWA" />
<!-- 앱의 이름 -->
<link rel="apple-touch-icon" href="/images/icons/icon-152x152.png" />
<!-- 아이콘 -->
```

### SEO 문제 해결을 위한 description 설정

SPA로 만들어진 웹앱의 경우 검색에 잘 걸리지 않는다는 문제가 있기 때문에, 정적 페이지에 검색에 걸릴 수 있는 키워드를 기재해준다.

``` html
<meta name="description" content="A sample weather app" />
```

### 주 테마 컬러 설정

```html
<meta name="theme-color" content="#2F3BA2" />
```

### 브라우저의 서비스워커 지원 여부 확인

```javascript
if ("serviceWorker" in navigator) {
  // 브라우저 안의 네비게이터가 서비스 워커를 지원한다면
  window.addEventListener("load", () => {
    // 로드될 때 해당 함수를 실행한다.
    navigator.serviceWorker.register("/service-worker.js").then(reg => {
      // 브라우저의 네비게이터에 서비스워커를 쓰겠다고 등록
    });
  });
}
```

## `service-worker.js`

라이프 사이클은 다음과 같다.

![](https://codelabs.developers.google.com/codelabs/your-first-pwapp/img/72ed77b1720512da.png)

### 캐시 등록 (install)

서비스가 실행되자마자 발생하는 이벤트, `install`에 캐시를 등록한다.

```javascript
const CACHE_NAME = "static-cache-v1";
// 버전관리를 위해 v~ 형식의 이름을 넣는다.
const FILES_TO_CACHE = [
	'/offline.html',
];

self.addEventListener("install", evt => {evt.waitUntil(
  caches.open(CACHE_NAME).then((cache) => {
    // 설정한 이름의 캐시를 오픈 후
    return cache.addAll(FILES_TO_CACHE);
    // 캐싱할 페이지를 넣는다.
  	})
	);
	self.skipWaiting();
});
```

만약 디바이스가 네트워크에 연결되어있지 않다면, 특정 페이지를 보여줘야 한다. 이 때 사용할 페이지를 미리 사용자의 캐시에 캐싱해놓을 필요가 있다.

#### `cache.open()`

사용자의 디바이스 내 캐시를 연다. 인자로 준 문자열로 캐시가 오픈된다.

#### `cache.addAll()`

캐시가 열렸을 때 호출할 수 있는 메서드로, 이 경우 url, 혹은 url 목록을 캐시 응답으로 추가 가능하다.

**목록으로 인자를 보낼 경우**

```javascript
const FILES_TO_CACHE = [
  "/",
  "/index.html",
  "/1.html",
  "/2.html",
  "/3.html",
];

self.addEventListener("install", evt => {
  evt.waitUntil(
  caches.open(CACHE_NAME).then((cache) => {
    return cache.addAll(FILES_TO_CACHE);
  	})
	);
	self.skipWaiting();
});
```

도중에 하나라도 캐싱에 실패하면 모두 캐싱이 되지 않는다. 캐싱이 에러로 인해 실패할 경우 다시 웹앱의 서비스워커가 동작할 때 캐싱이 실행된다.

### 캐시 관리 및 정리 (activate)

서비스 워커가 시작할 때마다 마찬가지로 `activate` 이벤트도 실행되는데, 사용자의 어플리케이션 활용에 필요한 리소스 및 네트워크 처리를 준비하고 이제는 사용하지 않는 리소스를 삭제한다.

```javascript
self.addEventListener("activate", evt => {
  // activate 이벤트 발생 시 실행할 이벤트 리스너 등록
  evt.waitUntil(
    caches.keys().then(keyList => {
      return Promise.all(
        keyList.map(key => {
          if (key !== CACHE_NAME) {
            // 캐시의 키가 변했다면
            return caches.delete(key);
            // 캐시 삭제
          }
        })
      );
    })
  );
  self.clients.claim();
});
```

> **`self.skipWaiting()`으로 시작해서 `self.clients.claim()`으로 종료될 때까지 서비스 워커는 해당 웹앱의 통제권을 가진다**

### 서비스워커의 요청 핸들링

#### fetch 로직 1

![](https://codelabs.developers.google.com/codelabs/your-first-pwapp/img/6302ad4ba8460944.png)

클라이언트가 요청의 요청이 네트워크로 바로 가는 기존 웹과 달리, 서비스워커는 해당 요청을 가로 챌 수 있다. 만약 해당 요청에 응답할 수 있는 데이터를 캐시가 가지고 있다면 클라이언트의 서비스워커가 응답한다.

```javascript
self.addEventListener("fetch", evt => {
  if (evt.request.mode !== "navigate") {
    // 만약 단순한 페이지 이동이라면 가로채지 않고 네트워크로 요청을 전송하며
    return;
  }
  evt.respondWith(
    // 응답한다
    fetch(evt.request).catch(() => {
      // 사용자의 요청을 가로채서
      return caches.open(CACHE_NAME).then(cache => {
        // 캐시를 열어서
        return cache.match("offline.html");
        // 해당 페이지를
      });
    })
  );
```
> `evt.responsewith()`를 사용하면 네트워크에서 요청을 처리하지 않고, 서비스 워커 단에서 응답하겠다는 뜻

#### fetch 로직 2

![](https://codelabs.developers.google.com/codelabs/your-first-pwapp/img/6ebb2681eb1f58cb.png)

클라이언트의 요청에 네트워크가 응답하면 해당 값이 클라이언트와 캐시에 들어온다. 이런 로직으로 구현을 해놓으면, 오프라인 상태의 사용자는 오래된 데이터나마 더 빨리 화면에서 확인이 가능하고, 네트워크가 응답했을 때 최신의 결과물을 볼 수 있다. 이렇게 쓰기 위해서는 캐시 단과 네크워트 단, **두 개의 비동기 요청**이 필요하다.

```javascript
function updateData() {
  Object.keys(weatherApp.selectedLocations).forEach(key => {
    const location = weatherApp.selectedLocations[key];
    const card = getForecastCard(location);
    getForecastFromCache(location.geo)
    // 캐시 관련 함수 호출
    .then((forecast) => {
      renderForecast(card, forecast);
    });
    getForecastFromNetwork(location.geo).then(forecast => {
      // 네트워크 관련 함수 호출
      renderForecast(card, forecast);
    });
  });
}
```

```javascript
 * @param {string} coords Location object to.
 * @return {Object} The weather forecast, if the request fails, return null.
 
function getForecastFromCache(coords) {
  if (!("caches" in window)) {
    // 윈도우에 캐시가 있는지 없는지 확인
    return null;
  }
  const url = `${window.location.origin}/forecast/${coords}`;
  // 캐시가 있다면 데이터를 가지고 옴
  return caches
    .match(url)
    .then(response => {
      if (response) {
        return response.json();
      }
      return null;
    })
    .catch(err => {
      console.error("Error getting data from cache", err);
      return null;
    });
}
```

```javascript
function renderForecast(card, data) {
  if (!data) {
    // 데이터가 없으면 업데이트 미실행
    return;
  }

  const cardLastUpdatedElem = card.querySelector(".card-last-updated");
  // 업데이트 정보를 지닌 노드를 가져와서
  const cardLastUpdated = cardLastUpdatedElem.textContent;
 	// 가지고 있는 데이터를 가져오고
  const lastUpdated = parseInt(cardLastUpdated);
	// 데이터 안에 있는, 타임스태프를 가져옴

  if (lastUpdated >= data.currently.time) {
  // 만약 업데이트가 지금 되었다면 아래의 로직을 실행하지 않음
    return;
  }
	
	// 중략
	
}
```

#### `html`외의 파일도 캐싱해놓기

> 리액트의 경우 번들만 캐싱해놓으면 된다

```javascript
const FILES_TO_CACHE = [
  '/',
  '/index.html',
  '/scripts/app.js',
  '/styles/inline.css',
  '/images/add.svg',
];

const CACHE_NAME = 'static-cache-v2';
const DATA_CACHE_NAME = 'data-cache-v1';
// 캐싱할 파일이 바뀌면 버전을 바꿔준다
```

##### `activate` 이벤트 리스너

조건문을 `key !== CACHE_NAME && key !== DATA_CACHE_NAME` 으로 바꿔준다. 즉 **키 이름이 캐시 이름, 데이터 캐시 이름과 같지 않은** 으로 조건을 바꾼다.

##### `fetch` 이벤트 리스너

`html` 뿐만 아니라 웹앱에 쓰이는 `js`, `css`, `svg` 까지 캐싱해두었기 때문에, 페이지 이동의 경우에도 캐시에 요청을 해야하는 상황이다. 때문에 `evt.request.mode !== "navigate"`라는 조건문을 삭제해야 한다.

```javascript
self.addEventListener("fetch", evt => {
	if (evt.request.url.includes("/forecast/")) {
    console.log("[Service Worker] Fetch (data)", evt.request.url);
    evt.respondWith(
      caches.open(DATA_CACHE_NAME).then(cache => {
        return fetch(evt.request)
          .then(response => {
            // 만약 네트워크 상황이 좋으면
            if (response.status === 200) {
              cache.put(evt.request.url, response.clone());
              // 캐시에 해당 응답을 넣는다
            }
            return response;
          })
          .catch(err => {
            // 네트워크 상황이 좋지 않으면
            return cache.match(evt.request);
            // 기존에 넣어두었던 캐시로 응답한다
          });
      })
    );
    return;
  }
  evt.respondWith(
    caches.open(CACHE_NAME).then(cache => {
      return cache.match(evt.request).then(response => {
        return response || fetch(evt.request);
      });
    })
  );
})
```

## 설치

### `index.html`

```html
<script src="/scripts/install.js"></script>
```

### `install.js`

```javascript
window.addEventListener('beforeinstallprompt', saveBeforeInstallPromptEvent);
// 설치 창이 뜨기 전 실행할 이벤트리스너 등록
let deferredInstallPrompt = null;
// 향후 event가 들어옴
const installButton = document.getElementById("butInstall");
// 설치 버튼을 html 노드에서 가져오고
installButton.addEventListener("click", installPWA);
// 해당 버튼이 클릭되면 실행될 이벤트리스너 등록
window.addEventListener('appinstalled', logAppInstalled);
// 설치가 되면 실행할 리스너
```

```javascript
function saveBeforeInstallPromptEvent(evt) {
  deferredInstallPrompt = evt;
  // event 객체 대입하고
  installButton.removeAttribute("hidden");
  // 감춰놨던 설치 버튼, 감춤 속성을 삭제
}
```

```javascript
function installPWA(evt) {
  deferredInstallPrompt.prompt();
  // 설치 프롬프트 만들기
  evt.srcElement.setAttribute("hidden", true);
  // 설치 버튼 숨기기

  deferredInstallPrompt.userChoice.then(choice => {
    // 유저가 이미 선택했다면 다시는 보이지 않도록 처리
    // 유저의 선택을 저장
    if (choice.outcome === "accepted") {
      console.log("User accepted the A2HS prompt", choice);
    } else {
      console.log("User dismissed the A2HS prompt", choice);
    }
    deferredInstallPrompt = null;
  });
}
```

```javascript
function logAppInstalled(evt) {
  console.log("Weather App was installed.", evt);
}
```

