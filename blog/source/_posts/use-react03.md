---
title: 리액트를 다루는 기술03 - css
date: 2019-02-19 09:47:18
tags:
categories:
- 개발공부
- React
---

> 모든 출처는 [벨로퍼티 님 블로그](https://velog.io/@velopert/react-component-styling)
>
> 보면서 따라한 내용 / 혼자 검색해서 볼 것 같은 내용을 정리

# SCSS

Sass를 CSS로 변환해주는 `node-sass`라는 라이브러리를 설치한다.

```
$ yarn add node-sass
```

```scss
// 변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;
// mixin 만들기 (재사용되는 스타일 블록을 함수처럼 사용 할 수 있음)
@mixin square($size) {
  $calculated: 32px * $size;
  width: $calculated;
  height: $calculated;
}

.SassComponent {
  display: flex;
  .box {
    background: red; // 일반 CSS 에선 .SassComponent .box 와 마찬가지
    cursor: pointer;
    transition: all 0.3s ease-in;
    &.red { 
      // .red 클래스가 .box 와 함께 사용 됐을 때
      // &는 부모를 레퍼런스할 때 쓴다.
      background: $red;
      @include square(1);
      // mixin 사용
    }
    &.orange {
      background: $orange;
      @include square(2);
    }
    &.yellow {
      background: $yellow;
      @include square(3);
    }
    &.green {
      background: $green;
      @include square(4);
    }
    &.blue {
      background: $blue;
      @include square(5);
    }
    &.indigo {
      background: $indigo;
      @include square(6);
    }
    &.violet {
      background: $violet;
      @include square(7);
    }
    &:hover {
      // .box 에 마우스 올렸을 때
      background: black;
    }
  }
}
```

## sass-loader 설정 커스터마이징

경로가 너무 길어질 경우 `Webpack`에서 `sass-loader` 설정을 커스터마이징하여 해결할 수 있다.

이 때 `create-react-app`으로 만들어진 것은 세부 설정들이 숨어있으므로 명령어를 통해 설정을 밖으로 꺼내준다.

```
yarn eject
```

원본

```javascript
            {
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction
                    ? shouldUseSourceMap
                    : isEnvDevelopment,
                },
                'sass-loader'
              ),
              sideEffects: true,
            }
```

교체

```javascript
            {
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders({
                importLoaders: 2,
                sourceMap: isEnvProduction && shouldUseSourceMap
              }).concat({
                loader: require.resolve('sass-loader'),
                options: {
                  includePaths: [paths.appSrc + '/styles'],
                  sourceMap: isEnvProduction && shouldUseSourceMap
                }
              }),
              sideEffects: true
            }
```

이 설정을 하게 되면 해당 파일 경로가 어디에 위치하더라도, 상대 경로를 입력하지 않고 `styles` 디렉토리 기준 절대경로로 불러올 수 있다.

만약 모든 곳에서 `import`가 필요한 css라면 옵션 설정을 해준다.

```scss
{
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders({
                importLoaders: 2,
                sourceMap: isEnvProduction && shouldUseSourceMap
              }).concat({
                loader: require.resolve('sass-loader'),
                options: {
                  includePaths: [paths.appSrc + '/styles'],
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                  data: `@import 'utils';`
                }
              }),
              sideEffects: true
            },
```

## node_modules 에서 불러오기

`open-color`나 `include-media` 등의 모듈을 사용했을 경우, 관련 `scss` 파일들이 `node_modules` 에 있다. 이를 가져오는 방법!

```scss
@import'~include-media/dist/include-media';
```

# CSS Module

CSS 클래스를 불러와서 사용 할 때 [js 파일이름]_[클래스이름]__[해쉬값] 형태로 클래스네임을 자동으로 고유한 값으로 만들어주어서 컴포넌트 스타일 중첩현상을 방지해주는 기술

이를 사용하기 위해서는 파일 이름을 `JS 파일 이름.module.css` 이런 식으로 저장해야 함

```scss
/* 자동으로 고유해질 것이므로 흔히 사용되는 단어를 클래스 이름으로 마음대로 사용가능*/

.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

.inverted {
  color: black;
  background: white;
}

/* 글로벌 CSS 를 작성하고 싶다면 */

:global {
    .something {
	  font-weight: 800;
	  color: aqua;
	}
}
```

`src/CSSModule.js`

```react
import React from 'react';
import styles from './CSSModule.module.css';

const CSSModule = () => {
  return (
    <div className={`${styles.wrapper} ${styles.inverted}`}>
      안녕하세요, 저는 <span className="something">CSS Module!</span>
    </div>
  );
};

export default CSSModule;
```

이 때 `{sytles.wrapper}`에 반환되는 객체는 `{wrapper: "CSSModule_wrapper__CUMkx"}`로 CSS Module 에서 사용한 클래스 이름과, 해당 이름을 고유화시킨 값이 key-value 형태로 들어있다.

## 만약 `CSS Module`이 아닌 파일에서 `CSS Module`을 쓰고 싶다면

```scss
:local {
  .wrapper {
    /* 스타일 */
  }
}
```

# classNames

 CSS 클래스를 조건부로 설정할 때 쓰는 라이브러리

```
$ yarn add classnames
```

기본 사용법

```javascript
import classNames from 'classnames';

classNames('one', 'two'); // 'one two'
classNames('one', { two: true }); // 'one two'
classNames('one', { two: false }); // 'one'
classNames('one', ['two', 'three']); // 'one two three'

const myClass = 'hello';
classNames('one', myClass, { myCondition: true }); //'one hello myCondition'
```

```javascript
const MyComponent = ({ highlighted, theme }) => {
  <div className={classNames('MyComponent', { highlighted }, theme)}>Hello</div>
}
```

`highlighted` 값이 뭔가에 따라 적용 여부가 달라진다.

## `classnames/bind`

```javascript
import React from 'react';
import classNames from 'classnames/bind';
import styles from './CSSModule.module.css';

const cx = classNames.bind(styles); // 미리 styles 에서 클래스를 받아오도록 설정하고

const CSSModule = () => {
  return (
    <div className={cx('wrapper', 'inverted')}>
      안녕하세요, 저는 <span className="something">CSS Module!</span>
    </div>
  );
};

export default CSSModule;
```

> # Tagged Template Literal
>
> 내부에 JavaScript 객체나 함수가 전달 될 때 이를 따로 추출할 때 사용
>
> ```javascript
> `hello ${{foo: 'bar' }} ${() => 'world'}!`
> // 결과: "hello [object Object] () => 'world'!"
> ```
>
> 문자열이 출력되어야 하는 곳에 `[object Object]`라고 나온다.
>
> 그래서 사용하는 방법
>
> ```javascript
> function tagged(...args) {
>     console.log(args);
> }
> tagged`hello ${{foo: 'bar' }} ${() => 'world'}!`
> ```

# styled-components

 하나의 자바스크립트 파일안에 스타일까지 작성 할 수 있다는 장점이 있다.

```javascript
import React from 'react';
import styled, { css } from 'styled-components';

const Box = styled.div`
  /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
  background: ${props => props.color || 'blue'};
  padding: 1rem;
  display: flex;
  /* 기본적으로는 1024px 에 가운데 정렬을 하고
  가로 크기가 작아짐에 따라 사이즈를 줄이고
  768px 미만으로 되면 꽉 채웁니다 */
width: 1024px;
margin: 0 auto;
@media (max-width: 1024px) {
  width: 768px;
}
@media (max-width: 768px) {
  width: 100%;
}
`;

const Button = styled.button`
  background: white;
  color: black;
  border-radius: 4px;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  font-size: 1rem;
  font-weight: 600;

  /* & 문자를 사용하여 Sass 처럼 자기 자신 선택 가능 */
  &:hover {
    background: rgba(255, 255, 255, 0.9);
  }

  /* 다음 코드는 inverted 값이 true 일 때 특정 스타일을 부여해줍니다. */
  ${props =>
        props.inverted &&
        css`
      background: none;
      border: 2px solid white;
      color: white;
      &:hover {
        background: white;
        color: black;
      }
    `};
  & + button {
    margin-left: 1rem;
  }
`;

const StyledComponent = () => (
    <Box color="black">
        <Button>안녕하세요</Button>
        <Button inverted={true}>테두리만</Button>
    </Box>
);

export default StyledComponent;
```

box의 media css의 경우 이렇게도 넣을 수 있다.

```javascript
import React from 'react';
import styled, { css } from 'styled-components';

const sizes = {
  desktop: 1024,
  tablet: 768
};

// 위에있는 size 객체에 따라 자동으로 media 쿼리 함수를 만들어줍니다.
// 참고: https://www.styled-components.com/docs/advanced#media-templates
const media = Object.keys(sizes).reduce((acc, label) => {
  acc[label] = (...args) => css`
    @media (max-width: ${sizes[label] / 16}em) {
      ${css(...args)};
    }
  `;

  return acc;
}, {});

const Box = styled.div`
  /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
  background: ${props => props.color || 'blue'};
  padding: 1rem;
  display: flex;
  width: 1024px;
  margin: 0 auto;
  ${media.desktop`width: 768px;`}
  ${media.tablet`width: 768px;`};
`;
```

스타일링 된 엘리먼트를 만들 때는

```react
import styled from 'styled-components';

const MyComponent = styled.div`
  font-size: 2rem;
`;
```

혹은 태그가 유동적이거나 특정 컴포넌트에 스타일링을 해야하는 상황이라면

(특정 컴포넌트에 스타일링을 해야하는 상황이 무슨 뜻이지)

```javascript
// 문자열로 styled 의 인자로 전달
const MyInput = styled('input')`
  background: gray;
`
// 아예 컴포넌트 형식의 값을 넣어줌
const StyledLink = styled(Link)`
  color: blue;
`
```

