---
title: maven / jdbc / restfulAPI
date: 2019-07-23 19:24:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# maven

![](https://cphinf.pstatic.net/mooc/20180723_93/1532327705849hiuxX_PNG/_.PNG)

- 편리한 의존성 라이브러리 관리를 돕는 도구
- `pom.xml `이라는 파일이 생긴다

> **CoC(Convention over Configuration)**
>
> - 일종의 관습
>   - 프로그램의 소스파일은 어떤 위치
>   - 소스가 컴파일된 파일들은 어떤 위치
>   - etc

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>kr.or.connect</groupId>
    <artifactId>examples</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>mysample</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

- `<project>` : 최상위 루트 엘리먼트(`Root Element`)
- `<modelVersion>` : `POM model` 버전
- `<groupId>`
  - 프로젝트를 생성하는 조직의 고유 아이디를 결정
  - *일반적으로 도메인 이름을 거꾸로 적는다*
- `<artifactId>` 
  - 해당 프로젝트에 의하여 생성되는 `artifact`의 고유 아이디를 결정
  - `Maven`을 이용하여 `pom.xml`을 빌드할 경우 다음과 같은 규칙으로 `artifact`가 생성
    - **`artifactid-version.packaging`**
    -  위 예의 경우 빌드할 경우 `examples-1.0-SNAPSHOT.jar` 파일이 생성
- `packaging` 
  - 해당 프로젝트를 어떤 형태로 packaging 할 것인지 결정
  - `jar`, `war`, `ear` 등이 해당
- `version` 
  - 프로젝트의 현재 버전
  - 프로젝트가 개발 중일 때는 `SNAPSHOT`을 접미사로 사용
- `name` : 프로젝트의 이름
- `url` : 프로젝트 사이트가 있다면 사이트 `URL`을 등록하는 것이 가능

> maven으로 프로젝트 생성 시 `WAS` 런타임 지정을 해줘야 함
>
> ```xml
> <dependency>
>    <groupId>javax.servlet</groupId>
>    <artifactId>javax.servlet-api</artifactId>
>    <version>3.1.0</version>
>    <scope>provided</scope>
> </dependency>
> ```
>
> **`<scope>`**
>
> - `compile`
>
>   - 컴파일 할 때 필요
>
>   - 테스트 및 런타임에도 클래스 패스에 포함
>   - scope 을 설정하지 않는 경우 기본값
>
> - `runtime` 
>
>   - 런타임에 필요
>     - *JDBC 드라이버 등이 예*
>   - 컴파일 시에는 필요하지 않지만, 실행 시에 필요한 경우
>
> - `provided`
>
>   - 컴파일 시에 필요하지만, 실제 런타임 때에는 컨테이너 같은 것에서 제공되는 모듈
>   - *`servlet`, `jsp`, `api` 등이 이에 해당*
>   - 배포 시 제외
>
> - `test` 
>
>   - 테스트 코드를 컴파일 할 때 필요
>   - 테스트 시 클래스 패스에 포함
>   - 배포 시 제외

## JSTL라이브러리

기본적으로 설정되어있지 않기 때문에 의존성 기재가 필요하다

```xml
<dependency>
   <groupId>javax.servlet</groupId>
   <artifactId>jstl</artifactId>
   <version>1.2</version>
</dependency>
```

## `web.xml`

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/webapp_3_1.xsd" version="3.1">
  <display-name>Archetype Created Web Application</display-name>
</web-app>
```

## `.settings/org.eclipse.wst.common.project.facet.core.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="jst.web" version="3.1"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
  <installed facet="java" version="1.8"/>
</faceted-project>
```

> # 웹 어플리케이션 초기화
>
> 1. 기존 `WAS`을 종료
> 2. 프로젝트를 우측 버튼으로 선택하고, Maven 메뉴 아래의 `update project`를 선택
> 3. `Servers view`에서 기존 `Tomcat Runtime`을 삭제
> 4. `Project` 메뉴의 `Clean` 선택
> 5. 프로젝트 익스플로러에서 `Server` 삭제

# JDBC

- `Java Database Connectivity`
- 자바를 이용한 데이터베이스 접속과 `SQL` 문장의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약
- 자바 프로그램 내에서 `SQL`문을 실행하기 위한 자바 `API`
- `SQL`과 프로그래밍 언어의 통합 접근 중 한 형태
- `JAVA`는 표준 인터페이스인 `JDBC API`를 제공
- 데이터베이스 벤더, 또는 기타 써드파티에서는 `JDBC `인터페이스를 구현한 드라이버(`driver`)를 제공

## 환경 구성

- JDK 설치

- JDBC 드라이버 설치

  ```xml
  <dependency>   
    <groupId>mysql</groupId>   
         <artifactId>mysql-connector-java</artifactId>
         <version>5.1.45</version>
   </dependency>
  ```

## JDBC를 이용한 프로그래밍 방법

![](https://cphinf.pstatic.net/mooc/20180201_49/1517475141729UGWfv_PNG/2_11_1_JDBC_.PNG)

1. `import java.sql.*;`

2. 드라이버 로드

   - `Class.forName( "com.mysql.jdbc.Driver" );`

3. `Connection` 객체를 생성

   ```java
   String dburl  = "jdbc:mysql://localhost/dbName";
   
   Connection con =  DriverManager.getConnection ( dburl, ID, PWD );
   ```

   > **예제**
   >
   > ```java
   > public static Connection getConnection() throws Exception{
   > 	String url = "jdbc:oracle:thin:@117.16.46.111:1521:xe";
   > 	String user = "smu";
   > 	String password = "smu";
   > 	Connection conn = null;
   > 	Class.forName("oracle.jdbc.driver.OracleDriver");
   > 	conn = DriverManager.getConnection(url, user, password);
   > 	return conn;
   > }
   > ```

4. `Statement` 객체를 생성 및 질의 수행

   1. 객체 생성

      `Statement stmt = con.createStatement();`

   2. 질의 수행

      ```java
      ResultSet rs = stmt.executeQuery("select no from user" );
      
      stmt.execute(“query”);             //any SQL
      stmt.executeQuery(“query”);     //SELECT
      stmt.executeUpdate(“query”);   //INSERT, UPDATE, DELETE
      ```

5. `SQL`문에 결과물이 있다면 `ResultSet `객체를 생성

   ```java
   ResultSet rs =  stmt.executeQuery( "select no from user" );
   while ( rs.next() )
         System.out.println( rs.getInt( "no") );
   ```

6. 모든 객체를 닫기

   ```java
   rs.close();
   stmt.close();
   con.close();
   ```

# Rest API

- REpresentational State Transfer

- REST 형식의 API
  - 핵심 컨텐츠 및 기능을 외부 사이트에서 활용할 수 있도록 제공되는 인터페이스
- 매시업(`Mashup`)
  - 서비스 업체들이 다양한 `REST API`를 제공함으로써, 클라이언트는 이러한 `REST API`들을 조합한 어플리케이션을 만듦

> ## API
>
> - Application Programming Interface
>
> - 주로 **파일 제어**, **창 제어**, **화상 처리**, **문자 제어** 등을 위한 인터페이스를 제공

## REST의 규칙

- `client-server`
- `stateless`
- `cache`
- `code-on-demand` (`optional`)
- `layered system`
- `uniform interface`

HTTP 프로토콜을 사용한다면 `client-server`, `stateless`, `cache`, `lared system`, `code-on-demand` 등에 대해서는 모두 쉽게 구현 가능

**문제는 `uniform interface`**

### `uniform interface`

- 리소스가 `URI`로 식별
- 리소스를 생성,수정,추가하고자 할 때 `HTTP` 메시지에 표현을 해서 전송
- **메시지는 스스로 설명 (`Self-descriptive message`)**
- **애플리케이션의 상태는 `Hyperlink`를 이용해 전이(`HATEOAS`)**

> 두 개가 가장 어렵다

## Web API(혹은 HTTP API)

REST의 `uniform interface`를 지원하는 것은 쉽지 않기 때문에, 많은 서비스가 `REST`에서 바라는 것을 모두 지원하지 않고 API를 만든다.

**REST의 모든 것을 제공하지 않고 Web API 혹은 HTTP API라고 부르는 경우**가 있다.

![](https://cphinf.pstatic.net/mooc/20180206_109/1517904573515UkVsl_PNG/2_11_2_webapi.PNG)

- `URI`는 정보의 자원을 표현
- 자원에 대한 행위는 `HTTP Method`(`GET`, `POST`, `PUT`, `DELETE`)로 표현
  - `GET` `/members`
    - 위의 표현은 맴버의 모든 정보를 달라는 요청
  - `DELETE` `/members/1`
    - `HTTP Method` 중의 하나인 `DELETE`를 이용하여 삭제를 표현
- 슬래시 구분자(`/`)는 계층을 나타낼 때 사용
  - 하이픈(`-`)은 URI가독성을 높일 때 사용
  - 언더바(`_`)는 사용하지 않는다
  - URI경로는 소문자만 사용
  - `RFC 3986`(`URI` 문법 형식)은 `URI` 스키마와 호스트를 제외하고는 대소문자를 구별
  - 파일 확장자는 `URI`에 미포함
  - `Accept Header`를 사용

### 상태 코드

![](https://cphinf.pstatic.net/mooc/20180206_273/1517904784161eGNFk_PNG/2_11_1_1.PNG)

![](https://cphinf.pstatic.net/mooc/20180206_113/1517904803278oyxuH_PNG/2_11_1_2.PNG)

![](https://cphinf.pstatic.net/mooc/20180206_194/1517904834300fxqr9_PNG/2_11_1_3.PNG)

### 실습

#### `pom.xml`

- `jdbc` 라이브러리 추가
- `json` 라이브러리 추가
- 서블릿 `api` 라이브러리 추가
- `jstl` 라이브러리 추가

> 업데이트 프로젝트

#### `Navigator` > `.settings` > `org.eclipse... core.xml`

`version`을 3.1로 바꿔줄 것

> 이클립스 재시작

#### `web.xml`

- `<failOnMissingWebXml>`을 `false`로 바꿔준다
  - 일반적으로 웹 어플리케이션 첫 시작 시 `web.xml`을 찾기 때문에, 이를 찾지 않을 것을 명기

#### `servlet`

- `response.setContentType`에 `application/json`을 인자로 넣어준다
- 해당 요청에 `json`으로 응답하겠다는 뜻

##### `ObjectMapper`

`json` 문자열로 바꾸거나, `json` 문자열을 객체로 바꾸는 역할 수행

##### `writeValueAsString`

파라미터로 `list`를 넣으면 리스트가 `json`으로 바뀌어 리턴됨