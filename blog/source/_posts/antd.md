---
title: antd
date: 2019-05-29 12:16:18
tags:
categories:
- 개발공부
- React
---

# antd

페이스북 이노베이션 랩 리액트 부트 캠프에서도 그렇고, 우아한형제들 테크러닝에서도 지속적으로 사용 중인 UI 라이브러리, antd. 해당 라이브러리는 **css와 js가 분리**되어 있어 각각 임포트가 필요하다는 특징이 있다.

## 레이아웃

### 그리드

출판 시스템에서 출발하였으며, 일반적으로 반응형 웹을 만들 때 주로 쓰인다. 보통 한 줄 당 12칼럼을 지닌 게 일반적이나, antd는 24칼럼을 가지고 있다.

```
<Row>
    {/* 로우로 시작 */}
    <Col span={6}>
    {/* 1~24 */}
        <Num>1</Num>
    </Col>
    <Col span={6}>
    {/* 이 안에서도 Row를 부르게 되면, 안의 Col은 부모를 24로 나눈 값으로 정렬된다 */}
    <Num>2</Num>
    </Col>
    <Col span={6}>
        <Num>3</Num>
    </Col>
    <Col span={6}>
        <Num>4</Num>
    </Col>
</Row>
```

#### offset

```html
<Row>
      <Col span={8}>col-8</Col>
      <Col span={8} offset={8}>
        col-8
      </Col>
</Row>
```

`offset` 속성을 가진 것은 중간에 여백을 가질 수 있다. 자세한 사항은 [요기](https://ant.design/components/grid/#components-grid-demo-offset)

#### Gutter

`Col` 사이에 여백을 주는 것도 가능하다.

```html
<Row gutter={16}>
  <Col span={6} />
  <Col span={6} />
  <Col span={6} />
  <Col span={6} />
</Row>
```

`gutter`는 픽셀 값을 지닌다.

### 플렉스

플렉스 컨테이너와 플렉스 아이템으로 구성된다. 플렉스는 브라우저 호환성을 고민해봐야 하는 문제인데, 플렉스 요소를 지닌 하위 요소들은 자동으로 플렉스 속성을 상속 받는다.

재밌는 점은, 원래 그리드 시스템과 플렉스는 혼용되기 어려운데, antd에서는 같이 쓸 수 있도록(*...?*) 구현해놨다는 점.

상위 Row에 플렉스 속성을 걸어 쓴다.

```html
<Row type="flex" justify="start">
      <Col span={4}>col-4</Col>
      <Col span={4}>col-4</Col>
      <Col span={4}>col-4</Col>
      <Col span={4}>col-4</Col>
</Row>
```

`justify`에 들어갈 수 있는 값 뿐만 아니라, Row가 가질 수 있는 속성들 또한 많다. 자세한 사항은 [요기](https://ant.design/components/grid/#components-grid-demo-flex)

> antd 디자인의 컴포넌트들에 어떤 요소가 있고, 뭘 적용했는지에 대해 굳이 필기를 할 필요를 느끼질 못해서(공식 홈페이지에 영문이지만 다 기술되어 있어서, 향후에 쓰더라도 그걸 보면서 개발하는 게 나을 것 같다는 생각을 했다.) 

