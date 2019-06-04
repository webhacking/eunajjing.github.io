---
title: 우아한테크러닝 Typescript & React 101 05 ANTD
date: 2019-06-04 12:17:18
tags:
categories:
- 개발공부
- 우아한테크러닝(React+Typescript)
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

> ~~antd 디자인의 컴포넌트들에 어떤 요소가 있고, 뭘 적용했는지에 대해 굳이 필기를 할 필요를 느끼질 못해서(공식 홈페이지에 영문이지만 다 기술되어 있어서, 향후에 쓰더라도 그걸 보면서 개발하는 게 나을 것 같다는 생각을 했다.)~~ 는 개뿔 그냥 하나하나 서술..

## 컴포넌트

### Menu

![](/images/woowahan05/menu.png)

`Menu.Item` 나  `SubMenu` 라는 하위 컴포넌트를 **반드시 가지며** 하위 컴포넌트들은 배열 형태를 취하기 때문에 반드시 `key`라는 속성을 지닌다.

#### 속성들

##### `theme`

Menu 컴포넌트 뿐만 아니라, 해당 컴포넌트 자손 요소의 색에도 영향을 주는 속성. 

##### `defaultSelectedKeys`

처음 페이지가 렌더링되었을 때 기본으로 선택될 메뉴를 지정하는 것으로, 값으로 **배열**이 들어간다. 메뉴는 실제로 배열 형태를 취하기 때문에, 해당 배열의 키를 서술해준다. 

#### 자손들

##### `Menu.Item`

하위 요소는 자동으로 중앙이다.

```html
<Menu.Item key="dashboard">
  <Icon type="dashboard" />
  대시보드
</Menu.Item>
```

##### `SubMenu`

```html
<SubMenu key="order"
  title={
    <span>
      <Icon type="rise" />
      거래 관리
    </span>
  }
{/*리액트는 속성에 리액트 컴포넌트를 기술할 수 있다.*/}
>
  <Menu.Item key="1">주문 시스템</Menu.Item>
  <Menu.Item key="2">정산 시스템</Menu.Item>
</SubMenu>
```

### Drawer

![](/images/woowahan05/drawer.png)

```html
<Drawer title="알림" placement="right" closable={false} visible={false}>
  <p>Some contents...</p>
  <p>Some contents...</p>
  <p>Some contents...</p>
</Drawer>
```

`visible` 은 열림 여부로, 차후 사용자 인터렉션에 따라 제어한다.

### Content

#### PageHeader

![](/images/woowahan05/pageHeader.png)

```html
<PageHeader title={<span>대시보드</span>}
  extra={[
    <Input.Search/>,
    <Button/>,
    <Button>
      <Avatar/>
    </Button>,
    <Button>
      <Badge>
        <Icon/>
      </Badge>
    </Button>
  ]}
/>
```

컴포넌트들의 각종 속성들은 임의로 생략 표기. `PageHeader` 의 `extra` 속성에 배열로 컴포넌트를 넣을 수 있고, 배열 안 컴포넌트들은 자동으로 우측 정렬된다.

### form

#### export 단

```javascript
export const Login = Form.create({ name: "loginForm" })(LoginComponent);
```

antd가 제공하는 form을 사용하기 위해서는, `Form.create` 를 사용하여 컴포넌트 래핑이 필요하다. 폼에서 쓰이는 다양한 도구들을 제공한다.

객체 형태로 들어가는 요소들을 다양한데, 그 중 예시로 쓰인 `name` 은 form 구성 컴포넌트의 id 접두사를 붙이는데 사용된다. 예를 들어, username을 입력할 수 있는 input 박스의 경우 Id는 `loginForm_inputId` 로 생성된다.

> 더 자세한 사항을 보고 싶다면 [공홈보다 더 나은 건 없지](https://ant.design/components/form/?locale=en-US#Form.create(options))

#### type 정의

```javascript
export interface ILoginComponentProps {
  authentication: IAuthentication | null;
  requestLogin(username: string, password: string): void;
  // 액션을 디스패치 해야하므로 함수를 만들어준다.
}

interface IProps {
  form: any;
}
```

#### form 구현

```jsx
class LoginComponent extends React.PureComponent<IProps & ILoginComponentProps> {
  onSubmit = event => {
  	event.preventDefault();
		this.props.form.validateFields((err, values) => {
  	  // 파라미터로 들어오는 values에는
    	// form 요소의 데이터들이 들어있으며, 객체로 이루어져있다.
	  	if (err) return;
  		this.props.requestLogin(values.username, values.password);
  		// 액션 디스패치
		});
	};

	render() {
		const { getFieldDecorator } = this.props.form;
 	 // 폼에서 제공하는 함수로 폼 요소에 대한 유효성 검사, 에러 메시지 출력 등을 수행
 	 // 함수를 리턴하는 특성이 있다, 자세한 사항은 아래 참고

		if (this.props.authentication) return <Redirect to={{ pathname: "/"}} />
 	 // 만약 로그인이 되어있으면 리다이랙트

	return (
  	<Card>
    	<Form onSubmit={this.onSubmit}>
      	<Form.Item>
        	{*/id절 생략/*}
      	</Form.Item>
      	<Form.Item>
       	 {getFieldDecorator("password", {
       	   rules: [{ required: true, message: "비번좀..." }]
       	 })(
       	   <Input prefix={<Icon type="lock"/>} type="password" placeholder="Password" />
      	  )}
      	</Form.Item>
      	<Form.Item>
        	{getFieldDecorator("remember", {
         	 valuePropName: "checked",
          	initialValue: true
   	     })(<Checkbox>나 좀 기억해줘</Checkbox>)}
    	    <br />
      	  <Button>로그인</Button>
      	</Form.Item>
    	</Form>
  	</Card>
	);
	}
}
```

##### `getFieldDecorator("id", {option})(<component/>)`

- `option`

  - `rules` : 유효성 검사 규칙으로, 배열이 들어간다.

    ```jsx
    <Form.Item>
      {getFieldDecorator("username", {
        rules: [{ required: true, message: "아이디좀..." }]
      })(
        <Input prefix={ <Icon type="user"/> } placeholder="Username" />
      )}
    </Form.Item>
    ```

  - `valuePropName` : `input`이 아닐 때 기재하는 옵션으로, 어떤 요소인지를 쓴다.

  - `initialValue` : 기본값으로 `value`를 지니므로 `input` 같은 경우에는 기재하지 않아도 된다.

  ```jsx
  <Form.Item>
    {getFieldDecorator("remember", {
     valuePropName: "checked",
      initialValue: true
   	})(<Checkbox>나 좀 기억해줘</Checkbox>)}
    <br />
    <Button>로그인</Button>
  </Form.Item>
  ```

- `function(<component/>)`

  - 위에 서술했다시피 해당 함수는 함수를 리턴한다.
  - 함수의 인자로 컴포넌트를 넣는다.

> ## 상위에서는 어떻게 state와 dispath 메서드를 넣어주나
>
> ```javascript
> import * as React from "react";
> import { connect } from "react-redux";
> import { IStoreState } from "../store";
> import { requestLogin } from "../actions";
> import { Login as LoginComponent, ILoginComponentProps } from "../components";
> 
> const Login: React.FC<ILoginComponentProps> = props => (
>   <LoginComponent {...props} />
> );
> 
> export default connect(
>   (state: IStoreState) => ({
>     authentication: state.authentication
>   }),
>   dispatch => ({
>     requestLogin: (username: string, password: string) =>
>       dispatch(requestLogin(username, password))
>   })
> )(Login);
> ```

