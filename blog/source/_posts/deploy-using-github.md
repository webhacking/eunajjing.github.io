---
title: 깃허브에 클라이언트 단 파일 올리기
date: 2019-04-28 20:44:00
tags:
categories:
- 개발공부
---

# 깃허브를 이용해 html, css, js 파일 호스팅

완성된 파일에 `yarn build` 명령어를 이용해 최적화한다.

`package.json`  파일에 `hompage`라는 필드를 만들어준다.

```javascript
"homepage": "https://<github-username>.github.io/<project-repo>"
```

`gh-pages` 플러그인을 설치한다.

`yarn add gh-pages -D`

`package.json`의 `scripts` 필드에 필드를 추가한다.

```javascript
"deploy" : "npm run build&&gh-pages -d build"
```

cmd에 `yarn run deploy` 명령어 실행