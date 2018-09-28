---
layout: 정보처리기사
title: 정보처리기사-DB_2
date: 2018-09-28 20:01:27
tags:
categories:
- 개발공부
- 정보처리기사
---
# SQL

----
## DDL
### 제약조건
- primary key
- unique
- foreign key ~ references ~
- check(속성명 = 범위 값)

----
- constraint 제역조건명 제약조건(속성명)

### 스키마 정의
create schema 스키마명 authorization 유저명;

### 도메인 정의
create domain 도메인명 데이터타입;
>도메인에도 속성에 줄 수 있는 check 제약조건을 똑같이 명시 가능
>>(디폴트 값, check(value in('값', '값')))

### 인덱스 정의
create index 인덱스명 on 테이블명(속성명)

----
## 외래키 지정 옵션
- on delete
- on update

----
- cascade
- set null
- set default
- no action

----
## DML (헷갈리는 것 위주로 용어만 정리)
- order by(asc, desc)
- exists
: 부속 질의문의 검색 결과 존재 여부에 따라 메인 쿼리 진행 여부가 달라짐

----
## DCL
- grant
: grant 권한내용 on 테이블명 to 유저명 [with grant option]
- with grant option
: 자신이 가진 권한을 다른 유저에게도 부여가 가능하다
- revoke
: revoke 권한내용 on 테이블명 from 유저명 [cascade]


>만약 권한을 취소 당한 유저가 다른 유저에게 해당 권한을 부여했다면, 그 권한 또한 연쇄적으로 취소된다.
