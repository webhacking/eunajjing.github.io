---
title: redirect&forward/Scope
date: 2019-07-22 20:40:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

## `redirect` & `forward`

### `redirect`

![](https://cphinf.pstatic.net/mooc/20180127_5/1517046342330PRbSX_PNG/2_4_1_redirect__.PNG)

- `서버`가 `클라이언트`의 요청에 대해 **특정 URL로 이동을 요청**하는 것
- `HTTP 프로토콜`로 정해진 규칙
- 서버는 클라이언트에게 HTTP 상태코드 `302`로 응답
  - 헤더 내 `Location` 값에 이동할 `URL` 을 추가
  - 클라이언트는 리다이렉션 응답을 받게 되면 헤더(`Location`)에 포함된 `URL`로 재요청
    - **클라이언트는 요청을 두 번 보내는 셈**
      - **이 때의 `request` 객체와 `response` 객체는 첫 요청과는 다른 객체**
  - 이때 브라우저의 주소창은 새 `URL`로 바뀜
- `서블릿`이나 `JSP`는 리다이렉트하기 위해 `HttpServletResponse` 클래스의 `sendRedirect()` 메소드를 사용
  - `sendRedirect()`의 인자로 리다이랙트할  `url` 을 넣는다

### `forward`

![](https://cphinf.pstatic.net/mooc/20180129_279/1517202070933x0x42_PNG/2_4_2_forward.png)

- `클라이언트`에서 `Servlet1`에게 요청을 보냄

- `Servlet1`은 요청을 처리한 후 결과를 `HttpServletRequest`에 저장

  - 처리한 결과를 타 서블릿에서 사용하기 위해서는, 전달되는 객체에 저장해야 하기 때문
  - 최초 요청 받은 서블릿이 일정한 부분만 처리하여 타 서블릿에 넘겨준다

- `HttpServletRequest`가 가지고 있는 `RequestDispatcher` 를 얻어낸다

  ```java
  RequestDispatcher req_dispatcher = request.getRequestDispatcher("/경로");
  ```
  - 포워드 경로는 반드시 **`/`**로 시작해야 한다

- `RequestDispatcher` 객체의 `forward()` 메서드에 `request`와 `response`를 인자로 넣어준다.
  - 즉, `request` 객체와 `response` 객체는 모든 서블릿에서 동일하다
    - 클라이언트 단 **`url`이 바뀌지 않는다**

- `Servlet2`는 받은 `HttpServletRequest`와 `HttpServletResponse`를 이용하여 요청을 처리한 후 클라이언트에게 응답

### `servlet` &` jsp` 연동

 `Servlet`에서 프로그램 로직이 수행되고, 결과를 `JSP`에게 포워딩하는 방법이 사용

![](https://cphinf.pstatic.net/mooc/20180129_201/1517203743283AcQbB_PNG/2_4_3_servlet_jsp.PNG)

## `Scope`

![](https://cphinf.pstatic.net/mooc/20180129_297/1517205425406SvaC6_JPEG/2_5_1_scope_.jpg)

### `page scope`

- 페이지 내에서 지역변수처럼 사용

- `PageContext` 추상 클래스를 사용
- `JSP `페이지에서 `pageContext`라는 내장 객체로 사용 가능
- `forward`가 될 경우 해당 `Page scope`에 지정된 변수는 사용 불가
  - 새로운 `PageContext`가 생성
- `jsp`에서 `pageScope`에 값을 저장한 후 해당 값을 EL표기법 등에서 사용할 때 사용

```jsp
<% pageContext.setAttribute("blah", a);
pageContext.getAttribute("blah"); %>
```

### `request scope`

- `http 요청`을 `WAS`가 받아서 웹 브라우저에게 응답할 때까지 변수값을 유지하고자 할 경우 사용
  - `request`, `response` 객체는 `WAS`가 만든다
- 서블릿에서는 `HttpServletRequest` 객체를 사용
- `JSP`에서는 `request` 내장 변수를 사용
  - 값을 저장할 때는 `request` 객체의 `setAttribute()`메소드를 사용
  - 값을 읽어 들일 때는 `request` 객체의 `getAttribute()` 메소드를 사용

### `session scope`

- 클라이언트 별로 변수를 관리하고자 할 경우 사용
- 웹 브라우저간의 탭 간에는 세션 정보가 공유되기 때문에, 각각의 탭에서는 같은 세션정보를 사용할 수 있음
  - *장바구니처럼 사용자별로 유지가 되어야 할 정보가 있을 때 사용*
- `HttpSession` 인터페이스를 구현한 객체를 사용
- `JSP`에서는 `session` 내장 변수를 사용
- `서블릿`에서는 `HttpServletRequest`의 `getSession()`메소드를 이용하여 `session` 객체를 얻는다
  - 값을 저장할 때는 `session` 객체의 `setAttribute()` 메소드를 사용
  - 값을 읽어 들일 때는 `session` 객체의 `getAttribute()` 메소드를 사용

- 세션 객체는 없어지지 않는다
  - 적당한 시간을 지정하여 자동 소멸 시키거나
  - 브라우저를 닫을 시 사라지게 만든다

### `application scope`

- **웹 어플리케이션이 시작되고 종료될 때까지 변수를 사용**할 수 있음
- 웹 어플리케이션 하나당 하나의 `application` 객체가 사용됨
-  모든 클라이언트가 공통으로 사용해야 할 값들이 있을 때 사용
- `ServletContext` 인터페이스를 구현한 객체 사용
  - `jsp`에서는 `application` 내장 객체를 이용
  - `서블릿`의 경우는 `getServletContext()` 메소드를 이용하여 `application` 객체를 이용
- 값을 저장할 때는 `application`객체의 `setAttribute()`메소드를 사용
- 값을 읽어 들일 때는 `application`객체의 `getAttribute()` 메소드를 사용

