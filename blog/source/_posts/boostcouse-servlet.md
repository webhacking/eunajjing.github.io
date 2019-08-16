---
title: Back-End
date: 2019-07-17 20:36:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# Back-End

## 개발 환경 설정

### JDK

`JAVA` 언어로 작성된 소스를 컴파일하고 관리하는데 사용하는 도구로, 컴파일한 결과를 실행하기 위해 `JRE`가 포함되어 있다.

#### JRE(Java SE Runtime Environment)

`JAVA` 언어로 작성된 프로그램을 실행하기 위한 프로그램

#### JDK가 OS 별로 설치 파일을 제공하는 이유

`JAVA`는 운영체제에 독립적으로 작동한다. 독립적으로 파일이 실행되기 위해서는 실행되는 환경 즉, `JRE`가 제대고 갖춰져 있어야 하며, 이 때문에 `JDK`를 설치할 때 운영체제별로 다르게 설치해야 한다.

#### JAVA 환경설정

시스템 환경 설정을 하는 방법은 운영체제에 따라서 다르다. 

### Tomcat

- 아파치 소프트웨어 재단에서 개발한 WAS(Web Application Server)
- 오픈소스

- 기본적으로 `8080` 포트로 실행된다

### 서블릿 컴파일 및 실행

#### 시작하기 전에

- `Target runtime` 설정
  - 최초 사용 시 `WAS`로 사용할 톰캣을 지정하되, 향후 만들 웹 어플리케이션은 기존 runtime을 사용하도록 설정하는 게 편하다

#### 서블릿

- `url` 요청을 처리하는 프로그램
- 서블릿 생성 시 `url mappings` 확인이 중요하다
  - `프로토콜://ip(서버 도메인):포트/프로젝트명/url mapping`

##### `doGet(HttpServletRequest, HttpServletResponse)`

- 클라이언트가 `get` 메서드 요청 시 호출되는 메서드

###### `HttpServletResponse`

- 응답할 내용을 모아 추상화해놓은 객체

- `HttpServletResponse.setContentType()`

  - 클라이언트에게 응답 결과를 알려줄 용도로, 이 타입을 보고 브라우저가 해석

    ```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response) {
    	response.setContextType("text/html;charset=UTF-8");
      // 보내는 text는 html 파일이다
      // 해당 파일의 인코딩은 utf-8
    }
    ```

- `HttpServletResponse.getWriter()`

  - `PrintWriter` 객체 리턴

- `PrintWriter("")`

  - 파라미터 안에 `html ` 태그를 넣을 수도 있다

## Servlet

- 자바 웹 어플리케이션의 구성요소 중 동적인 처리를 하는 프로그램의 역할
- `WAS`에 동작하는 `JAVA` 클래스
- 서블릿은 HttpServlet 클래스를 상속
- 대개
  - 웹 페이지를 구성하는 화면(HTML)은 JSP로 표현
  - 복잡한 프로그래밍은 서블릿으로 구현

### 자바 웹 어플리케이션의 폴더 구조

![](https://cphinf.pstatic.net/mooc/20180124_133/15167752967943AqfC_PNG/1_5_1_____.PNG)

#### `WEB-INF`

##### `web.xml`

- 해당 웹 어플리케이션에 대한 정보들을 다 지니고 있는 파일
  - 클라이언트가 요청할 때 해당 `URL`로 요청을 하게 되면 `servlet-name`이 같은 서블릿을 찾아 특정 패키지 안에 있는 서블릿 실행
- `web.xml`이 변경되면 서버는 반드시 재시작되어야 한다
- servlet 3.0 미만에서는 필수 파일이었으나 3.0 이상은 어노테이션으로 기재도 가능

##### `lib`

- 각종 자료 파일들을 넣는다

##### `classes`

- 실제 `class`들 (`java` 패키지 같은)

> #### 파일 경로
>
> 워크스페이스 > `.metadata` > `.plugins` > `org.eclipse.wst.server.core` > `tmp0`
>
> ##### `tmp0`
>
> - 이클립스가 톰캣을 사용하기 때문에
> - 톰캣의 디렉토리 구조와 거의 유사
>
> `tmp0` > `wtpwebapps` > `프로젝트들`

### Servlet 작성 방법

> ### server
>
> - 클라이언트 요청 시 두 개의 객체가 자동으로 생성된다
>   - 요청을 받아내는 `request` 객체
>   - 응답을 하는 `response` 객체

#### Servlet 3.0 이상

- 자바 어노테이션 사용
  - `@WebServlet('/blahblah')`

#### Servlet 3.0 미만

- `Servlet` 등록 시 `web.xml`에 등록

  ```xml
   <servlet>
          <description></description>
          <display-name>TenServlet</display-name>
          <servlet-name>TenServlet</servlet-name>
          <servlet-class>exam.TenServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>TenServlet</servlet-name>
          <url-pattern>/ttt</url-pattern>
      </servlet-mapping>
  ```

### Servlet 라이프 사이클

#### 사용자가 만든 Servlet

`HttpServlet`의 3가지 메서드를 상속

- `init()`

- `service(request, response)`

  - `HttpServlet`의 `service` 메서드는 템플릿 메서드 패턴으로 구현

    > **템플릿 메서드 패턴**
    >
    > - 어떤 작업을 처리하는 일부를 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내용을 바꾸는 패턴
    >   - 동일 기능을 상위 클래스에서 정의하며 확장/변화가 필요한 부분만 서브 클래스에서 구현
    >
    > > 출처 : [[Design Pattern] 템플릿 메서드 패턴이란](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)

    - `doGet(request, response)`
    - `doPost(request, response)`

- `destroy()`

![](https://cphinf.pstatic.net/mooc/20180124_22/1516782982944xjogH_PNG/1_5_3_ServletLifcycle.PNG)

- `WAS`는 서블릿 요청을 받으면 해당 서블릿이 메모리에 있는지 확인
  - 메모리에 없다면
    - 해당 서블릿 클래스를 메모리에 올림
      - 객체가 생성되는 작업
    - `init()` 실행
  - 메모리에 있다면
    - `service()` 실행
      - 응답해야하는 모든 내용은 해당 메서드에 구현해야 함
  - `WAS`가 종료되거나 웹 어플리케이션 갱신의 경우 `destroy()` 메서드 실행

- 서블릿은 서버에 **서블릿 객체를 여러 개 만들지 않는다**

## `Request`, `Response` 객체의 이해

![](https://cphinf.pstatic.net/mooc/20180124_79/15167843899250uB2H_PNG/1_5_4_request_response.PNG)

### 클라이언트와 서버의 통신

- 브라우저는 도메인과 포트 번호를 이용해 서버에 접속
  - `path` 정보
  - 클라이언트 `ip` 등의 클라이언트 정보
- 클라이언트 정보를 서버에 전송

- `WAS`는 웹 브라우저로부터 `Servlet` 요청을 받으면
  - 요청할 때 가지고 온 정보를 `HttpServletRequest`객체를 생성하여 저장
  - 응답을 보낼 때 사용하기 위해 `HttpServletSponse`객체를 서블릿에게 전달
  - 생성된 두 객체를 서블릿에게 전달
    - `service()`, `doGet()`, `doPost()` 메서드의 파라미터로 전달되어 사용됨

### HttpServletRequest

- `http 프로토콜`의 `request` 정보를 서블릿에게 전달하기 위한 목적으로 사용
- `헤더 정보`, `파라미터`, `쿠키`, `URI`, `URL` 등의 정보를 읽어 들이는 메소드가 있다
  - `getHeaderNames()`
    - 반환받을 때 `Enumeration<String>`으로 반환
    - 여러 개의 요소를 가지고 있어 이터레이터로 요소를 가지고 있는지를 검사
    - 첫 번째 요소는 `headername` 이다
  - `getHeader(headername)`
    - `headerValue`를 반환
  - `getParameter("name")`
    - 해당 파라미터의 value를 반환
    - 모든 value는 **문자열로 반환**
  - `getRequestURI()`
    - 도메인과 포트 이하에 있는 값을 리턴
  - `getRemoteAddr()`
    - 요청한 클라이언트의 주소 등이 반환

> **URI**(Uniform Resource Identifier)
>
> - 정보 리소스를 고유하게 식별하고 위치를 지정
> - `url` 과 `urn` 등의 하위 개념을 포함
> - `WAS` 등을 처리하는 자원 식별자
>
> - `rewrite` 기술을 이용하여 만든 의미있는 식별자
>
> > **rewrite** 기술
> >
> > 요청을 통해서 주어진 URL의 규칙을 변경해서 웹서비스를 보다 유연하게 만드는 방법
> >
> > - 의미론적 url로의 변경
> > - 확장자의 치환
> > - 리다이렉션
> > - 내부 리다이렉션
> >
> > > 출처 : [재작성(rewrite)](https://www.opentutorials.org/module/384/4337)
>
> - rest 서비스
> - 웹 기반의 구조 문법
>
> **URL**(Uniform Resource Locator)
>
> - 특정 서버의 한 리소스에 대한 구체적인 위치를 서술
>
> **URN**(Uniform Resource Name)
>
> - 콘텐츠를 이루는 한 리소스에 대해, 그 리소스의 위치에 영향 받지 않는 유일무이한 이름 역할
> - 리소스를 여기저기로 옮기더라도 문제없이 동작
> - URL의 한계로 인해 착수
>
> > 출처
> >
> > - [URL과 URI는 무슨 차이일까?](https://jjunii486.tistory.com/28)
> > - [URI vs URL vs URN](https://mygumi.tistory.com/139)

- `Body`의 `Stream`을 읽어 들이는 메소드를 가지고 있다

  - `Stream`

    - 클라이언트와 서버 사이 맺어진 연결을 통해 양방향으로 주고받는 하나 이상의 `Message`
      - `Message`
        - 요청 / 응답의 단위로 다수의 `Frame`으로 이루어짐
          - `Frame`
            - `http/2` 통신 상 제일 작은 정보의 단위
            - `Header` 혹은 `Data`

    ![](https://t2.daumcdn.net/thumb/R720x0/?fname=http://t1.daumcdn.net/brunch/service/user/JqQ/image/z4szqsmS7gz_g3MS0LK49ZGh8JU.png)

    > 출처 : [HTTP/2에서 Frame, Stream의 의미](https://brunch.co.kr/@sangjinkang/3)

### HttpServletResponse

- 클라이언트에게 응답을 보내기 위한 `HttpServleResponse` 객체를 생성하여 서블릿에게 전달
- 서블릿은 `HttpServleResponse` 객체를 이용하여 `content type`, `응답코드`, `응답 메시지` 등을 전송