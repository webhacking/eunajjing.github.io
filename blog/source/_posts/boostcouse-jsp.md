---
title: JSP와 JSTL & EL
date: 2019-07-20 23:11:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# JSP와 JSTL & EL

## JSP

- 모든 `JSP`는 서블릿으로 바뀌어서 동작

### 지시자

- 지시자를 듣는 것은 `JSP`를 실행하는 `WAS`
- 지시자 안에 쓴 것들은 모두 서블릿의 `service()`에 들어와 변경이 된다
  - 응답에 포함되는 건 `Service()` 뿐이며, 매번 호출되기 때문 
  - 단, **`<%! %>` 선언식은 예외로 `Service()` 바깥에 코드들이 만들어진다**

#### 지시자의 종류

- `<&@ &>`

  - 페이지 지시자
    - 서블릿에서 `response`  객체에 `contentType`, `charset`을 지정했던 것과 같다

- `<% %>`
  - `Scriptlet`
  - 자바 코드 입력
    - 선언된 변수는 지역 변수
  - 서블릿으로 바뀔 적에 자바 코드로 변환된다
    - 주석을 사용한다면 이 또한 서블릿으로 들어간다

- `<%= %>`

  - 표현식
  - 서블릿으로 바뀔 적에 `out.print()`으로 바뀐다

  > **`out.print()`와 `System.out.print()`의 차이**
  >
  > - `out.print()`는 `response`한테 출력하는 것
  > - `System.out.print()` 는 콘솔에 출력하는 것
  > - **즉 출력하는 곳이 다름**

- `<%! %>`

  - 선언식
  - 보통 자바의 메서드나 전역변수 쓰고자 할 때 사용

- `<%-- --%>`

  - 주석 
  - 서블릿으로 바뀌지 않으며 브라우저 상의 소스에서도 표시되지 않는다
  - 반면 `html` 주석은 서블릿으로 바뀌며, 브라우저 상에서도 표시

### 라이프사이클

#### `JSP`의 실행순서

1. 브라우저가 웹서버에 `JSP`에 대한 `request` 정보를 전달
2. 브라우저가 요청한 `JSP`가 최초로 요청했을 경우만 `JSP`로 작성된 코드가 서블릿으로 코드로 변환(`java` 파일 생성)
3. 서블릿 코드를 컴파일해서 실행가능한 `bytecode`로 변환 (`class` 파일 생성)
4. 서블릿 클래스를 로딩하고 인스턴스를 생성
5. 서블릿이 실행되어 `request`을 처리하고 `response` 정보를 생성

### 내장 객체

- `_jspService()`에 삽입된 코드의 윗부분에 미리 선언된 객체들이 있는데, 해당 객체들은 jsp에서도 사용 가능

#### 종류

![](https://cphinf.pstatic.net/mooc/20180130_74/1517275973733EL11k_PNG/2_3_4_jsp_.PNG)

## EL(Expression Language)

값을 표현하는 데 사용되는 스크립트 언어로서 JSP의 기본 문법을 보완

- `html`  태그 사이에서도 사용 가능
- `JSTL` 사이에서도 사용 가능

### 기능

- `JSP`의 스코프(`scope`)에 맞는 속성 사용
- 집합 객체(ex.컬렉션)에 대한 접근 방법 제공
- 수치 연산, 관계 연산, 논리 연산자 제공
- 자바 클래스 메소드 호출 기능 제공
- 표현언어만의 기본 객체 제공

### 기본 객체

![](https://cphinf.pstatic.net/mooc/20180130_153/1517281495386qOuqH_PNG/2_6_1__.PNG)

### 표현방법

![](https://cphinf.pstatic.net/mooc/20180130_78/1517281954147RNccz_PNG/2_6_1__.PNG)

#### 객체 사용 예

![](https://cphinf.pstatic.net/mooc/20180130_68/1517282068498tAlQM_PNG/2_6_1____.PNG)

#### 객체 접근 규칙

![](https://cphinf.pstatic.net/mooc/20180130_27/1517286832617YwnDB_PNG/2_6_1_.PNG)

- 표현 1이나 표현 2가 `null`이면 `null`을 반환
- 표현1이 `Map`일 경우 표현2를 `key`로한 값을 반환
- 표현1이 `List`나 `배열`이면 표현2가 정수일 경우 해당 정수 번째 `index`에 해당하는 값을 반환
  - 만약 정수가 아닐 경우에는 오류
- 표현1이 객체일 경우는 표현2에 해당하는 `getter()`에 해당하는 메소드를 호출한 결과를 반환

### 데이터 타입

- `boolean`
- `int`
- `double`
- `String`
  - 따옴표(` '` 또는`"` )로 둘러싼 문자열
  - 만약 작은 따옴표(`'`)를 사용해서 표현할 경우 값에 포함된 작은 따옴표는 `\'` 와 같이 `\` 기호와 함께 사용
- `\` 기호 자체는 `\\` 로 표시
- `null`

### 연산자

#### 우선순위

1. `[]` 
2. `()`
3. `-`, `not`, `! `, `empty`
4. `* `,`/`, `div`, `%`, `mod`
5. `+`, ` -`
6. `<`,  `>`, `<=`, `>=`, `lt`, `gt`, `le`, `ge`
7. `==`, `!=`, `eq`, `ne`
8. `&&`, `and`
9. `||`, `or`
10. `? :`

#### 수치

- `+`, `-`, `*`, `/` or `div	`, `%` or `mod`
- 객체와 수치 연산자를 사용할 경우 객체를 숫자 값으로 변환 후 연산자를 수행 
  - `${"10"+1} → ${10+1}`
- 숫자로 변환할 수 없는 객체와 수치 연산자를 함께 사용하면 에러
- 수치 연산자에서 사용되는 객체가 null이면 0으로 처리
  - `${null + 1} → ${0+1}`

#### 비교

- `==` 또는 `eq`
- `!=` 또는 `ne`
- `<` 또는 `lt`
- `>` 또는 `gt`
- `<=` 또는 `le`
- `>=` 또는 `ge`
- 문자열 비교
  - `${str == '값'}`
  - `str.compareTo("값")`
  - ` == 0`

#### 논리

- `&&` 또는 `and`
- `||` 또는 `or`
- `!` 또는 `not`

#### empty, 3항 연산자

![](https://cphinf.pstatic.net/mooc/20180130_17/1517287228502gEf9g_PNG/2_6_1_empty_%2C__.PNG)

### 비활성화

페이지 지시자에 `isELIgnored`를 `true`로 기재한다.

서블릿 2.4 이상부터 기본 값은 `false`이다.

2.3 이하 버전에서는 `EL` 자체가 무시된다

`<%@ page isELIgnored = "true" %>`

## JSTL(JSP Standard Tag Library)

SP 페이지에서 조건문 처리, 반복문 처리 등을 html tag형태로 작성할 수 있게 돕는다.

![](https://cphinf.pstatic.net/mooc/20180130_149/1517289583487Ac0YJ_PNG/2_6_2_jstl.PNG)

### 사용법

- [다운로드 링크](http://tomcat.apache.org/download-taglibs.cgi)
  - `impl.jar`
  - `spec.jar`
  - `jstlel.jar`
- `WEB-INF/lib/` 폴더에 복사
- `taglib` 지시자를 사용하여 `prefix` 지정
  - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> `

![](https://cphinf.pstatic.net/mooc/20180130_273/1517290494334HrB7S_PNG/2_6_2_jstl___.PNG)

#### 코어

![](https://cphinf.pstatic.net/mooc/20180130_226/1517290578353rKRbE_PNG/2_6_2_jstl_.PNG)

##### 변수 지원

![](https://cphinf.pstatic.net/mooc/20180226_240/1519633482313pWfP8_PNG/1.png)

`session.setAttribute("varName", "someValue")`

![](https://cphinf.pstatic.net/mooc/20180226_103/1519633640114VKW2d_PNG/2.png)

##### 흐름제어

![](https://cphinf.pstatic.net/mooc/20180226_83/1519633710402BlJ2W_PNG/3.png)

- `test="조건"`
  - 조건 부분에는 보통 `EL` 태그가 쓰인다

![](https://cphinf.pstatic.net/mooc/20180130_4/1517292532220uxSVD_PNG/2_6_2__choose.PNG)

![](https://cphinf.pstatic.net/mooc/20180130_218/1517292735244tmWgM_PNG/2_6_2__forEach.PNG)

![](https://cphinf.pstatic.net/mooc/20180130_93/1517293018908uGgzT_PNG/2_6_2__import.PNG)

- `praram`은 `url` 상에서 쿼리스트링으로 보낼 것들을 명시해주는데 사용. 옵션
- 특정 페이지에 있는 모든 값을 읽어와 변수에 저장

![](https://cphinf.pstatic.net/mooc/20180130_170/1517293246119dFJ4F_PNG/2_6_2__redirect.PNG)

##### 기타 태그

![](https://cphinf.pstatic.net/mooc/20180130_55/1517293404340WP4J3_PNG/2_6_2__out.PNG)

- `escapeXml`의 기본 값은 `true`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%> 
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:set var="t" value="<script type='text/javascript'>alert(1);</script>" />
${t}
<c:out value="${t}" escapeXml="true" />
<!-- 이 경우 자바스크립트가 실행되지 않고, 자바스크립트가 문자열로 출력된다-->
<c:out value="${t}" escapeXml="false" />
<!-- 이 경우 자바스크립트가 실행되며, 자바스크립트가 문자열로 출력되지 않는다-->
</body>
</html>
```

