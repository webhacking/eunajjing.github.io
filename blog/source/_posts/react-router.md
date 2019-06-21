---
title: 리액트 라우터 복습
date: 2019-06-11 14:59:18
tags:
categories:
- 개발공부
- React
---

# 라우터 내 파라미터의 사용

## 라우터 컴포넌트의 `props`

- `history`

  - `push` : 리다이렉트
  - `replace`
  - `goBack` : 뒤로가기

  ```javascript
  import React from "react";
  
  const History = ({ history }) => {
  
    const goBack = history.goBack;
    const push = history.push("/");
  
    return (
  
      <div>
        <button onClick={goBack}>뒤로가기</button>
        <button onClick={push}>홈으로 이동</button>
      </div>
  
    );
  };
  
  export default History;
  ```

  ```javascript
  import React from "react";
  import qs from "qs";
  // 쿼리스트링 사용을 위해 패키지 설치
  
  const About = ({ location: { search } }) => {
    const query = qs.parse(search.substr(1));
    // 객체형태로 쿼리를 파싱
    // ?detail=true를 detail : true 로
  
    const detail = query.detail === "true";
    // 쿼리의 값이 원하는 값인지 확인하는 작업 필요
  
    return (
      <div>
        <h1>소개</h1>
        <p>About 컴포넌트입니다</p>
        {detail && <p>추가적인 정보</p>}
    		{*/detail이라는 쿼리가 들어와 해당 값이 true일 때만 컴포넌트가 보임/*}  
      </div>
    );
  };
  
  export default About;
  ```

- `match`

  - `prams`에 대한 정보를 가지고 있음

    (blahblah/:username)

- `location`

  - 현재 경로에 대한 정보를 지님
  - URL 쿼리를 가지고 있음

```javascript
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';
import Profile from './Profile';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
      <Route path="/profiles/:username" component={Profile} />
    </div>
  );
};

export default App;
```

`username`라는 쿼리를 받을 것. 즉 `localhost:3000/profiles/blahblah`라는 형태로 요청이 들어온다.

```javascript
import React from "react";
import { Link, Route, NavLink } from "react-router-dom";
import Profile from "./Profile";

const Profiles = () => {
  return (
    <div>
      <h3>유저 목록 : </h3>
      <ul>
        <li>
          <NavLink to="/profiles/1111" activeStyle={{ color: "black" }}>
            11111
          </NavLink>
        </li>
        <li>
          <NavLink to="/profiles/2222">2222</NavLink>
          {/* link와 navlink의 차이, navlink는 activeStyle, activeClassname 있음 */}
        </li>
      </ul>

      <Route
        exact
        path="/profiles"
        render={() => <div>유저를 선택해주세요</div>}
      />
      <Route path="/profiles/:username" component={Profile} />
    </div>
  );
};

export default Profiles;
```

```javascript
import React from "react";

const profileData = {
  "1": {
    name: "1111",
    description: "11111111111"
  },
  "2": {
    name: "2222",
    description: "222222222222"
  }
};

const Profile = ({match:{params:{username}}}) => {
  const profile = profileData[username];
  
  if (!profile) {
    return <div>user not exist!</div>;
  }
  return (
    <div>
      <h3>
        {username} - {profile.name}
      </h3>
      <p>{profile.description}</p>
    </div>
  );
};

export default Profile;
```

## 라우터 컴포넌트 외에서 라우터의 `props`를 쓸 때

```javascript
import React from 'react';
import { withRouter } from 'react-router-dom';
const WithRouterSample = ({ location, match, history }) => {
  return (
    <div>
      <h4>location</h4>
      <textarea value={JSON.stringify(location, null, 2)} />
      <h4>match</h4>
      <textarea value={JSON.stringify(match, null, 2)} />
      <button onClick={() => history.push('/')}>홈으로</button>
    </div>
  );
};

export default withRouter(WithRouterSample);
```

> `JSON.stringify()`
>
> 인자로 들어간 값이나 객체를 JSON 문자열로 반환