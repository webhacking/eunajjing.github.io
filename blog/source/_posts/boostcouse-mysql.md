---
title: DB 연결 웹 앱 - MYSQL
date: 2019-07-22 15:39:18
tags:
categories:
- 개발공부
- 부스트코스
- 웹 프로그래밍
---

# DB 연결 웹 앱 - MYSQL

## DB

- 데이터의 집합 (`a Set of Data`)
- 여러 응용 시스템(프로그램)들의 통합된 정보들을 저장하여 운영할 수 있는 공용(`share`) 데이터의 집합
- 효율적으로 저장, 검색, 갱신할 수 있도록 데이터 집합들끼리 연관시키고 조직화

### 특성

- 실시간 접근성(`Real-time Accessability`)
  - 사용자의 요구를 즉시 처리

- 계속적인 변화(`Continuous Evolution`)
  - 정확한 값을 유지하려고 삽입·삭제·수정 작업 등을 이용해 데이터를 지속적으로 갱신

- 동시 공유성(`Concurrent Sharing`)
  - 사용자마다 서로 다른 목적으로 사용하므로 동시에 여러 사람이 동일한 데이터에 접근하고 이용

- 내용 참조(`Content Reference`)
  - 저장한 데이터 레코드의 위치나 주소가 아닌 사용자가 요구하는 데이터의 내용, 즉 데이터 값에 따라 참조

## DBMS

- 데이터베이스를 관리하는 소프트웨어
- 여러 응용 소프트웨어(프로그램) 또는 시스템이 동시에 데이터베이스에 접근하여 사용
- 필수 3기능
  - 정의기능
    - 데이터 베이스의 논리적, 물리적 구조를 정의
  - 조작기능
    - 데이터를 검색, 삭제, 갱신, 삽입, 삭제하는 기능
  - 제어기능
    - 데이터베이스의 내용 정확성과 안전성을 유지하도록 제어하는 기능

### 장점

- 데이터 중복이 최소화
- 데이터의 일관성 및 무결성 유지 
- 데이터 보안 보장

### 단점

- 운영비가 비싸다
- 백업 및 복구에 대한 관리가 복잡
- 부분적 데이터베이스 손실이 전체 시스템을 정지

## SQL(`Structured Query Language`)

- SQL은 데이터를 보다 쉽게 검색하고 추가, 삭제, 수정 같은 조작을 할 수 있도록 고안된 컴퓨터 언어
- 관계형 데이터베이스에서 데이터를 조작하고 쿼리하는 표준 수단
- `DML` `(Data Manipulation Language`)
  - 데이터를 조작하기 위해 사용
    `INSERT`, `UPDATE`, `DELETE`, `SELECT` 등이 여기에 해당

- `DDL` (`Data Definition Language`)
  - 데이터베이스의 스키마를 정의하거나 조작하기 위해 사용
  - `CREATE`,` DROP`, `ALTER` 등이 여기에 해당

- `DCL` (`Data Control Language`)
  - 데이터를 제어하는 언어
  - 권한을 관리하고, 테이터의 보안, 무결성 등을 정의
  - `GRANT`, `REVOKE` 등이 여기에 해당

### DB 생성

1. 관리자 계정으로 접속 `mysql –uroot  -p`

2. DB 생성 `create database DB이름;`

### 사용자 생성 및 권한 할당

- `Database`를 생성했다면, 해당 데이터베이스를 사용하는 계정을 생성해야 함
- 해당 계정이 데이터베이스를 이용할 수 있는 권한을 부여해야 함
  - db이름 뒤의 `*` 는 모든 권한을 의미
  - `@’%’`는 어떤 클라이언트에서든 접근 가능하다는 의미
  -  `@’localhost’`는 해당 컴퓨터에서만 접근 가능하다는 의미
  - `flush privileges`는 DBMS에게 적용을 하라는 의미

```mysql
grant all privileges on db이름.* to 계정이름@'%' identified by ＇암호’;
grant all privileges on db이름.* to 계정이름@'localhost' identified by ＇암호’;
flush privileges;
```

### 접속

`mysql –h호스트명 –uDB계정명 –p 데이터베이스이름`

### 특징

- `SQL`은 `;`으로 끝난다
- `SQL`은 쿼리(`Query`)라고 읽는다
  - 쿼리는 대소문자를 가리지 않는다

- `SELECT`는 어떤 내용을 조회할 때 사용하는 키워드

- MySQL은 쿼리에 해당하는 결과의 전체 row를 출력하고 마지막에 전체 row 수와 쿼리실행에 걸린 시간을 표시
- 쿼리 작성 중 `₩c`를 사용하면 중간에 취소 가능

### 명령어

- `SHOW statement;`
  - `DBMS`에 존재하는 데이터베이스 확인
- `use mydb;`
  - 사용중인 데이터베이스 전환 
    - 데이터베이스를 전환하려면, 이미 데이터베이스가 존재
    - 현재 접속 중인 계정이 해당 데이터베이스를 사용할 수 있는 권한이 있어야 함

### `DDL`(`create`, `drop`)

#### table

![](https://cphinf.pstatic.net/mooc/20180131_276/1517366013781n0BtX_PNG/2_8_1_%28table%29_.PNG)

- `table`
  - `RDBMS`의 기본적 저장구조 한 개 이상의 `column`과 0개 이상의 `row`로 구성
- `Column`
  - 테이블 상에서의 단일 종류의 데이터
  - 특정 데이터 타입 및 크기를 지님
- `Row`
  - `Column`들의 값의 조합
  - 레코드
  - 기본키(`PK`)에 의해 구분
    - 기본키는 중복을 허용하지 않으며 없어서는 안 된다
- `Field`
  - `Row`와 `Column`의 교차점으로 `Field`는 데이터를 포함할 수 있고 없을 때는 NULL 값

> `Empty set` : DB에 아직 테이블이 생성되지 않았음
>
> `desc 테이블명;` : 테이블의 구조 확인

#### 데이터 타입

![](https://cphinf.pstatic.net/mooc/20180131_89/1517386938999sf3SM_PNG/2_8_3_MySQL__-1.PNG)

![](https://cphinf.pstatic.net/mooc/20180131_46/1517386952668I67cM_PNG/2_8_3_MySQL__-2.PNG)

> `AUTO_INCREMENT` : 자동으로 1씩 증가하는 번호

### `DML`(`select`, `insert`, `update`, `delete`)

![](https://cphinf.pstatic.net/mooc/20180131_187/1517374752273Ba8n9_PNG/2_8_2_select__.PNG)

> **오라클의 `dual`** 테이블
>
> 오라클에서는 반드시 테이블이 있어야 한다
>
> 하지만 테이블에 없는 데이터를 임시로 꺼낼 수도 있기 때문에, 해당 테이블을 **임시 테이블**로 지정해 값을 출력할 수 있다

#### where

![](https://cphinf.pstatic.net/mooc/20180227_23/1519695801630edbfc_PNG/3.PNG)

- `column in (blah, blah)`
- `column like '%a_'`

#### 함수

##### 단일함수

- `concat` : 문자열 결합 함수
  - `select concat(column, 'blah', column) from tablename`

- `UCASE`, `UPPER` : 대문자로 변경

- `LCASE`, `LOWER` : 소문자로 변경

- `substring('blahblah', n, n)`

  - DB는 `0`이 아닌 `1`부터 시작

- `LPAD`, `RPAD` : 원하는 문자열로 채움

  - `LPAD('hi',5,'?'), RPAD('joe',7,'*')`
    - `???hi `, `joe****`

- `TRIM`, `LTRIM`, `RTRIM` : 공백 삭제

- `ABS(x)` : 절대값 구하기

- `MOD(n,m)` : 나머지 구하기

- `FLOOR(x)` : `x`보다 크지 않은 가장 큰 정수를 반환

- `CEILING(x) `: `x`보다 작지 않은 가장 작은 정수를 반환

- `ROUND(x)` : `x`에 가장 근접한 정수를 반환

- `POW(x,y)`, `POWER(x,y)` : `x`의 `y` 제곱 승을 반환

- `GREATEST(x,y,...)` : 가장 큰 값을 반환

- `LEAST(x,y,...)` : 가장 작은 값을 반환

- `CURDATE()`, `CURRENT_DATE` : 오늘 날짜를` YYYY-MM-DD`나 `YYYYMMDD` 형식으로 반환

- `CURTIME()`, `CURRENT_TIME` : 현재 시각을 `HH:MM:SS`나 `HHMMSS` 형식으로 반환

- `NOW()`, `SYSDATE()` , `CURRENT_TIMESTAMP` : 오늘 현시각을` YYYY-MM-DD HH:MM:SS`나 `YYYYMMDDHHMMSS` 형식으로 반환

- `DATE_FORMAT(date,format)` : 입력된 `date`를 `format 형식`으로 반환합니다.

- `PERIOD_DIFF(p1,p2)` : `YYMM`이나 `YYYYMM`으로 표기되는 `p1`과 `p2`의 차이 개월을 반환

- `cast` 형 변환 (`mysql` 4.2 버전부터 지원)

  ![](https://cphinf.pstatic.net/mooc/20180227_7/1519696097137n1dmo_PNG/4.png)

##### 그룹함수

여러 개의 값을 가지고 결과 값 하나만 출력

![](https://cphinf.pstatic.net/mooc/20180131_87/151738015308653Cmb_PNG/2_8_2_select_%28%29.PNG)

![](https://cphinf.pstatic.net/mooc/20180227_237/15196955203980m2pE_PNG/2.PNG)

