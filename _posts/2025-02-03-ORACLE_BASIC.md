---
layout: post
title:  " [Oracle] 1. Oracle 시작하기(이론편) "
categories: Oracle
author: start-easy
---
* content
{:toc}

## PL/SQL

* SQL을 확장한 절차적 언어

* RDBMS에서 사용되는 Oracle의 표준 데이터 엑세스 언어로, 프로시저 생성자를 SQL과 완벽하게 통합한다.

* 유저 프로세스가 PL/SQL을 보내면 서버 프로세스는 PL/SQL Engine에서 해당 블록을 받아 SQL과 Procedural를 나눠 SQL은 SQL statement executer로 보낸다.

* PL/SQL 분류 : Procedure, Function, Trigger

---


### Select문 실행 순서

* FROM-WHERE-GROUP BY-HAVING-SELECT-ORDER BY 순으로 실행


``` shell
1. FROM : 발췌 대상 테이블 참조
2. WHERE : 발췌 대상 데이터가 아닌 것은 제거
3. GROUP BY : 행들을 소그룹화 한다.
4. HAVING :  그룹핑된 값의 조건에 맞는 것만을 출력한다.
5. SELECT : 데이터 값을 출력/계산한다.
6. ORDER BY :  데이터를 정렬한다.

```

### 메서드

* `TO_CHAR` : 문자열로 변환하는 함수

* `TO_DATE` : DATE형식으로 변경

> ex) TO_CHAR(TO_DATE(’1919/12/31’), ’YYYYMMDD’)

``` shell

SELECT
			A.SAL AS TSAL
			,NVL(A.COMM, 0) AS COMM
FROM
			EMP A
ORDER BY TSAL DESC;

# 결측치로 0으로 바꾸고 SELECT문을 실행한다.

```


* `LIKE` : 유사 형태에 맞춘 데이터 찾는 함수

``` shell

SELECT ENAME FROM EMP WHERE ENAME LIKE “_A%”;

# 2번째 글짜가 A인 모든 것을 찾는다.

```