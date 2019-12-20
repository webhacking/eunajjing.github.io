---
title: 쇼룸, 쇼핑 개발자를 부탁해
date: 2019-09-06 18:29:18
tags:
categories:
- 일상
---

# [쇼룸, 쇼핑 개발자를 부탁해](https://festa.io/my/tickets/event/440)

## 계기

## 세션 요약

### 쇼핑검색 Java/JSP에서 Node.js/React 전환기

- 김득중 님 발표

#### 하는 일

- 통합검색 및 쇼핑판 컨텐츠 노출
- 사내 타 서비스 제공
- 외부 제공

#### 왜 바꿨나요?

- `jsp`가 요구사항 반영이 어려웠다
  - 더이상의 `DOM` 조작은 싫었다
  - 타 팀의 영향으로 시작

#### 그래서

##### 공부

- es6 배우기
- react 배우기
- typescript 배우기
  - 데이터 다루기가 편리

##### 다른 사이트 참고

- 사용 시나리오 작성
  - 클라이언트 / 서버사이드 / 공통 로직은 어디에 위치해야 하는가?
- 컴포넌트 분리
- `redux` `store` 설계

##### 개발

- 성능 문제 발생
  - 서버
    - 서버사이드 `XML` 파서의 문제
      - `easysax`를 사용한 유틸리티 제작
    - 서버에서 클라이언트에 내려지는 데이터 사이즈가 기존보다 3~4배
      - `store` 데이터 최소화
      - `css` 단축
  - 클라이언트
    - 불필요한 리렌더링 방지
      - 불필요한 스토어 데이터를 `connect`하고 있지 않은지

- `apm`
  - `es-apm`

#### 깨알 팁

- 민감한 코드가 클라이언트 번들에 들어가지 않도록 주의
- 민감한 데이터가 스토어에 들억지 않도록 주의
- 가능한 `store` 줄이기
- `keep-ailve` 활성화

- `pr`로 시작하는 개발문화 바꾸기

node.js, typescript, react

### 브라우저 날개를 달자

- 이병승 님 발표

#### 브라우저를 가볍게 만드려면?

- resource size
- loop count
- logic

##### 네트워크 적으로 신경 써야 하는 것

- dns
- **cdn**
- idc/switch
- **http protocol**
  - request header
  - cookie

##### 서버에서 신경 써야 하는 것

- resource(io / cpu / memory)
- **cache**
- **logic**
- **non-blocking**
- **비동기 처리**

##### DB에서 신경 써야 하는 것

- lock, wait
- internal process mechanism
- **SQL Tuning**
- **Batch**

##### 브라우저

- 비동기 처리

- non-blocking
- network
- resource (img, javascript)
  - svg : gzip
  - png, jpg : webpack
- connetion count
- rendering



- conclusion
- 로드 시간에 집중할 것

### UI 모듈화로 워라밸 지키기

- 안정민 님 발표

#### 공통 컴포넌트 미리 정의

##### 장점

- 리소스 간단
- 유지보수 편리

##### 단점

- 영향 범위 파악이 어려움
  - `storybook` 활용

##### 종류

###### 마크업 같음 && stateful 로직 같음

컴포넌트 재활용이 간단

###### 마크업 다름, stateful 로직 같음

- 로직 공통화 필요
- 커스텀 훅 활용

###### 마크업 같음, stateful 로직 다름

- 래퍼 컴포넌트 활용
  - `props`의 전달의 전달 문제 발생
    - 코드 가독성이 떨어짐
  - `context` 적용
    - 전역으로 관리할 수 있는 `state`를 넣는다

###### 마크업 다름, stateful 로직 다름

공유할 수 없다

### `IaC`를 어쭙잖게 맛본 썰

- 최승호 님 발표



### `k8s`위에 `Node.js` 앱 하나를 올리면서 고민한 것들

- 정형식 님 발표

## 느낀점