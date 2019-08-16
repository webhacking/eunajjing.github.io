---
title: Front-End
date: 2019-07-16 15:39:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# Front-End

## HTML

### Tags

`HTML` 태그는 굉장히 많은 것들이 있고, 그것들은 모두 다 각각의 쓰임에 대해 **정확한 의미를 지니고 있다(semantic)**

- 필요한 태그를 찾아서 적절한 의미에 맞는 태그를 사용하는 것이 중요
- SEO에 영향을 준다

#### Layout Tags

![](https://cphinf.pstatic.net/mooc/20171231_41/15146999078486r8Pv_JPEG/5086.HTML5PageLayout_2.jpg)

- `header`, `footer`는 사실 `div` 태그와 같은 일을 하는데, **의미가 다르다**
- 몇몇의 태그들은 `html5`를 지원하지 않는 브라우저의 경우 제대로 동작하지 않을 가능성이 있다
  - 몇몇 라이브러리들이 변환을 해주는 작업을 수행한다

### `class`와 `id`

#### `id`

- 고유한 속성으로 한 HTML 문서에 **하나만** 사용 가능

#### `class`

- 하나의 HTML 문서 안에 **중복해서 사용 가능**
- 하나의 태그에 여러 개의 다른 class 이름을 공백을 기준으로 나열 가능

## CSS

### CSS의 구성

```css
selector {
	property : value;
}
```

### CSS 적용 방법

#### inline

```html
<tag style="property1:value1; property2:value2;"/>
```

- `html` 태그 안에 적용
- 다른 `css` 파일에 적용한 것보다 가장 먼저 적용됨

#### internal

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      selector {
        property : value;
      }
    </style>
  </head>
</html>
```

- `<style>` 태그로 지정
- 장점
  - 별도의 `css` 파일을 관리하지 않아도 된다
  - 서버에 `css` 파일 request를 보내지 않아도 된다
- 단점
  - 유지보수가 어렵다

#### external

```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="style.css"/>
  </head>
</html>
```

- 외부파일(`.css`)로 만드는 것
- 아주 짧은 코드가 아니라면 이 방법이 최선

### 상속과 우선순위

- 일반적으로 상위에서 적용한 스타일은 자손에게도 반영

```css
selector1 selector2 {
	property : value;
}
```

- ` `(띄어쓰기)는 자손 관계를 의미
  - `selector1`의 하위 요소인 `selector2`의 모든 하위 요소에 스타일 지정

```css
selector1 > selector2 {
	property : value;
}
```

- `>`는 자식 관계를 의미
  - `selector1`의 바로 직계 하위 요소인 `selector2`에 스타일 지정

```css
prent-selector > selector:nth-child(n) {
	property : value;
}
```

- `nth-child(n)`
  - `prent-selector`의 하위 직계 요소의 `n`번째 자식에게만 스타일 지정

```css
selector.class {
	property : value;
}
```

- 혼합
  - `selector.class`
    - 특정 셀렉터 중 특정 클래스에만 스타일 지정

#### box-model

- 크기, 배치 관련 속성은 **상속이 되지 않는다**
  (*`width`, `height`, `margin`, `padding`,`border`*)
- `block Element`의 경우 `box-moel` 속성이 사용된다

##### 엘리먼트의 크기

- 기본적으로 부모의 크기만큼 가진다

  ```css
  selector {
  	box-sizing:content-box;
    /* 이 경우 padding 속성에 따라 크기가 커질 수 있다 */
    box-sizing:border-box;
    /* 이 경우 테두리가 박스 사이즈임을 명기했기 때문에 padding 속성을 바꾼다고 해도 크기가 변경되지 않는다 */
  }
  ```

#### 일반적인 CSS

1. `liline`
2. 나중에 선언된 속성
   - `internal`과 `external`은 우선 순위가 동등

#### cascading 우선순위

- **구체적**인 것이 먼저 반영
- 같은 선택자라면 **나중에 선언된 것이 반영**

### CSS 기본 Style 변경

#### font

- `color`
- `font-size`

- `background`
- `font-family`

> ### `em` 단위
>
> 설정된 값 대비 상대적인 크기를 배수로 지정하는 것
>
> > 만약 `16px`이 기본 값이라면, `2em`은 `32px`이다.
>
> - **`font-size`는 상속이 된다**
>   - 즉 부모 노드에 상속된 `font-size`가 무엇이냐에 따라 자식 요소의 `em` 기준 값이 바뀐다

## CSS layout

### 렌더링 과정

- 엘리먼트를 화면에 배치하는 것
- 위에서 아래로, 순서대로 블록을 이루며 배치되는 것이 기본

### `display`

- 어떤 영역들을 가진 기본 속성
  - 위에서 아래로 떨어지는 속성
    - `block`
    - `inline-block`
      - `div`
      - `p`
  - 왼쪽에서 오른쪽으로 흐르는 속성
    - `inline`
      - `span`
      - `a`
      - `strong`

### `position`

- 특별한 값을 가지면서 배치가 되는 속성
  - `static`
    - 순서대로 배치
  - `absolute`
    - **상위 엘리먼트들 중에 `static` 속성을 가지지 않은 엘리먼트를 기준**으로 위치
    - 만약 상위의 모든 엘리먼트가 `static` 속성을 가지고 있다면, `body` 태그를 기준
  - `relative`
    - **원래 자신이 위치해야 할 곳을 기준**으로 상대적인 곳에 위치
  - `fixed`
    - `viewport` 를 기준으로 하여 위치

### `float`

- 기본적인 순서에서 벗어나 엘리먼트를 공중에 띄우는 속성

#### `overflow`

- `float` 속성을 지니고 있는 자식 요소를 인식하게 함

- 즉, **부모 엘리먼트에 사용**

  ```css
  selector {
  	overflow:auto;
    /* value로 hidden을 줘도 된다*/
  }
  ```

