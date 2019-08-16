---
title: post CSS
date: 2019-07-10 18:27:18
tags:
categories:
- 개발공부
- CSS
---

# POST CSS

JS를 편하게 쓰면서도, 향후 JS로 변환되는 수많은 JS 라이브러리와 같이 CSS를 편하게 작성하기 위한 새로운 문법 역시 지속적으로 제시되어왔다. 대표적인 CSS 전처리기로는 `Sass`, `Less`, `stylus` 등이 있다.

## 이해한 내용 정리

### CSS Pre-Processors

기존 CSS 문법의 확장 성격으로 자기만의 syntax로 작성하고 parse/compile 과정을 거쳐 일반 CSS로 되돌려준다.

#### `stylus`

예시로 든 세 개의 CSS 전처리 장치 중 가장 최신의 것으로, `node.js`로 구동된다. **문법이 굉장히 관대하여 초심자가 쓰기 어렵다**는 평이 많다. 단, 예시로 든 세 개의 전처리 장치 중 **기능이 제일 막강하다**고 평가되는 반면, **성숙도가 굉장히 낮다**.

#### `Less`

초기에는 `Sass`처럼 `ruby`로 구현되었으나, 현재는 `js`로 구현되어 있다. **브라우저 단에서 동작하며, 독특한 제어문 양식을 가진다.** 상대적으로 세 개의 CSS 전처리 장치 중 **기능이 적다**.

#### `Sass`

예시로 든 세 개의 CSS 전처리 장치 중 가장 오래된 것으로, **성숙도가 높은 반면 `ruby`환경으로 동작되어 느리다.**

- `Sass` 하나만을 위해 `ruby`를 추가하는 것은 굉장한 부담이다. 
  - `node-sass`를 사용하는 방법도 있다.
    - `node-gyp`에 의존
      - 로컬에서 안 깨져도 서버에서 깨지는 문제 발생
- `Future CSS`와 호환되지 않음
- `autoprefixer` 때문에 어차피 내부적으로 `postCSS`를 돌려 써야 함

### CSS Post-Processors

CSS 문법으로 작성된 것을 해석/처리해서 다시 일반 CSS를 돌려준다.

## 참고 레퍼런스

- [CSS를 작성하는 더 편한 방법: LESS와 SASS](https://taegon.kim/archives/3667)

- [내가 Sass를 선택한 이유](https://windtale.net/blog/why-i-choose-sass/)

- [PostCSS 소개](https://medium.com/@FourwingsY/postcss-소개-727310aa6505)
- [Post-Processor를 이용한 깔끔하고 미래 지향적인 CSS 작성](https://appletree.or.kr/blog/web-development/css/post-processor를-이용한-깔끔하고-미래-지향적인-css-작성/)

postCSS + CSSnext