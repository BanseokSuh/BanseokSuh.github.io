---
title: 쿼리 실행 계획, 옵티마이저
author: Banny
date: 2023-01-19 23:20:00 +0900
categories: [DB, MySQL]
tags: [optimizer]
---

## 쿼리 실행 계획이란?

- 하나의 문장으로 작성된 SQL 쿼리가 DB엔진 내부에서 어떻게 실행될 지를 수립하는 것
- 옵티마이저에 의해 실행된다.
- 실행 계획을 확인하는 방법은 EXPLAIN PLAN, AUTO TRACE, SQL TRACE의 3가지 방법이 있다.

## 옵티마이저 (Optimizer)

- SQL 쿼리문을 가장 효율적으로 수행할 최적의 루트를 생성해주는 DBMS의 핵심 엔진
- 아래 그림은 사용자가 작성한 쿼리문이 실행될 때의 절차이다.

<center>
<img alt="optimizer" src="https://user-images.githubusercontent.com/62047302/213447338-7cbb41e4-ce96-427b-b7f2-1ff7313ddfc5.png">
</center>

### 쿼리 실행 절차

1_SQL 파싱

- 사용자(개발자 혹은 DBA)가 작성한 SQL 쿼리문이 실행될 때, 구문은 SQL 파서로 전달이 된다.
- 이 단계에서 SQL 파서라는 모듈은 쿼리를 MySQL 서버가 이해할 수 있는 수준으로 분리한다.
- 쿼리가 문법적으로 잘못되었다면 이 단계에서 걸러진다.
- 파서에 의해 SQL 파스 트리가 만들어진다.
- MySQL 엔진은 파스 트리를 해석하여 나머지 절차가 실행된다.

<br>

2\_최적화 및 실행 계획 수립

- 옵티마이저는 파스 트리를 해석하여 어떤 테이블부터 읽고 어떤 인덱스를 이용할 지 선택하여 실행계획을 생성한다.
- 실행 계획을 세우는 방식에 따라 규칙 기반 옵티마이저와 비용 기반 옵티마이저로 나뉜다.
- 규칙 기반 옵티마이저는 옵티마이저에 내장된 우선순위에 따라 실행 계획을 수립하는 방식이다. 기준이 고정적이다.
- 비용 기반 옵티마이저는 여러 방법을 만들고, 대상 테이블의 통계 정보를 이용해서 실행 계획의 비용을 산출하여 가장 낮은 리소스를 사용하는 방식을 선택한다.
- 현재 대부분의 RDBMS가 비용 기반 옵티마이저를 채택한다고 한다.
<center>
<img alt="optimizer" src="https://user-images.githubusercontent.com/62047302/213447348-2b163db1-772e-48d3-bd02-1d8e0dc8beb4.png">
</center>

- 통계정보는 꾸준히 갱신되고 있는 것이 좋다.
- DBMS_STATS 패키지를 사용하면 통계 정보를 수집할 수 있다고 한다.
- 주요 통계 정보들은 다음과 같다.

<center>
<img alt="dictionary" src="https://user-images.githubusercontent.com/62047302/213453343-3d22ce4f-037c-4532-9480-b98d3f5594a8.png">
</center>

<br>

3\_스토리지 엔진으로부터 데이터 조회

- 수립된 실행 계획대로 SQL 실행 엔진은 데이터를 조회한다.

<br>
<br>
