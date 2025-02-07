---
layout: post
title:  " [Oracle] 1. Oracle 질의어(이론편) "
categories: Oracle
author: start-easy
---
* content
{:toc}

## 질의어 종류

* DQL(Data Query Language)

> 조회 및 검색 기능 : SELECT

``` shell
SELECT [distict] {*, column [alias], ...} from (table_name);

# alias(as) 는 생략할 수 있고, 별칭으로 지정할 수 있다.
# - 공백이나 특수문자를 적용하려면 "(더블 쿼터)를 사용해야 한다.
```

* DDL(Data MAnipulation Language)

> 저장, 수정, 삭제 : INSERT, UPDATE, DELETE
cf) 메모리에 적재된다. commit 하지 않으면 외부에서 조회가 불가능한 상태이다.

* TCL(Transaction Control Language)

> 데이터 영구 저장, 취소 : COMMIT, ROLLBACK,. SAVEPOINT

* DCL

> 시스템 권한 부여, 회수수 : GRANT, REVOKE

``` shell
CREATE TABLE 계정이름.테이블이름(
	칼럼명1 	데이터타입1(사이즈)
    ,칼럼명2	데이터타입2
    ,칼럼명3	데이터타입3(사이즈)
    , ...
    , 칼럼명N	데이터타입N(사이즈)
)

# 계정 이름 없을 시 현재 계정에 테이블 생성
```

예제

``` shell
CREATE TABLE CAT AS
SELECT * FROM ANIMAL WHERE 1=2;

# where 절이 false이므로 칼럼만 들고온다.
```
