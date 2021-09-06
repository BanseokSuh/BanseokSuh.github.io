---
title: 동시성 제어(Concurrency Control)
author: Banny
date: 2021-09-06 20:21:00 +0900
categories: [데이터베이스]
tags: [데이터베이스, 동시성 제어]
---

## 동시성 제어란?

<center>
<img alt="concurrency control" src="https://user-images.githubusercontent.com/62047302/132086194-b99362b8-3d0f-4a57-b1e8-9ffe7ea0f937.png">
</center>

<br>

> 동시에 실행되는 여러 개의 트랜잭션이 작업을 성공적으로 마칠 수 있도록 트랜잭션의 순서를 제어하는 기법입니다.<br>
> 그것의 목적은 트랜잭션의 직렬성을 보장하고, 데이터의 무결성 및 일관성을 보장하기 위함입니다.

<br>

## 동시성 제어를 하지 않는다면?

동시성 제어를 하지 않는다면 다음과 같은 일이 발생합니다.

<strong>1. 갱신 손실(Lost Update)</strong>

- 하나의 트랜잭션이 갱신한 내용을 다른 트랜잭션이 덮어씀으로써 갱신한 내용이 무효화되는 것을 의미합니다.
- 두 개 이상의 트랜잭션이 하나의 데이터를 동시에 update할 때 발생합니다.

<strong>2. 현황파악오류(Dirty Read)</strong>

- 읽기 작업을 하는 트랜잭션1이 쓰기 작업을 하는 트랩잭션2가 작업한 중간 데이터를 읽음으로써 발생합니다.
- 작업 중인 트랜잭션2가 작업을 Rollback한 경우에 트랜잭션은1은 무효화된 데이터를 읽게 됩니다.

<strong>3. 모순성(Inconsistency)</strong>

- 다른 트랜잭션들이 하나의 값을 갱신하는 동안, 어느 트랜잭션은 그것의 갱신 전의 값을 읽고, 다른 트랜잭션은 그것의 갱신 후의 값을 읽게 되어 데이터의 불일치가 발생하는 상황입니다.
- 즉 두 개의 트랜잭션이 동시에 실행될 때 DB가 일관성이 없는 모순된 상태로 남아있게 되는 문제를 초래합니다.

<strong>4. 연쇄복귀(Cascading Rollback)</strong>

- 두 개의 트랜잭션이 하나의 데이터에 접근할 때 발생합니다.
- 한 트랜잭션이 데이터를 갱신한 다음 실패하여 Rollback을 수행하는 과정에서, 갱신과 Rollback 연산 사이에 해당 데이터를 읽음으로써 발생할 수 있는 문제입니다.

<br>

## 동시성 제어 기법의 종류

<strong>1. 락킹(Locking)</strong>

<center>
<img alt="concurrency-control-locking" src="https://user-images.githubusercontent.com/62047302/132206842-4a5cccc2-3472-4fb5-9fe7-a3e377bf6585.png">
</center>

<br>

락킹은 하나의 트랜잭션이 실행하는 동안 특정 데이터 항목에 대해서 다른 트랜잭션이 동시에 접근하지 못하도록 상호배제 기능을 제공하는 기법입니다.
하나의 트랜잭션이 데이터 항목에 대해서 락킹을 설정하면, 잠금을 설정한 트랜잭션이 해제(unlock)할 때까지 데이터를 독점적으로 사용할 수 있습니다.

- S락(Shared Lock, 공유 잠금): S락을 설정한 트랜잭션은 데이터 항목에 대해 읽기(read) 연산만 가능합니다.
  예를 들어, T1에서 x에 대해 S-lock을 설정했다면, T1은 read(x) 연산만 가능합니다.

- X락(Exclusive Lock, 배타 잠금): X락을 설정한 트랜잭션은 데이터 항목에 대해서 읽기 연산(read)과 쓰기 연산(write) 모두 가능합니다.
  예를 들어, T1에서 x에 대해 S-lock을 설정했다면, T1은 read(x) 연산과 write(x) 연산 모두 가능합니다.

- 하지만 잠금의 한계로 교착상태(deadlock)가 발생할 수 있다고 합니다.
  > 교착상태(Deadlock): 다수의 트랜잭션이 특정 자원의 할당을 무한정 기다리고 있는 무한 대기 상태

<br>

<strong>2. 타임스탬프(Timestamp)</strong>

시스템에서 생성하는 고유 타임스탬프를 각 트랜잭션에 부여함으로써 작업 순서를 정합니다. 교착상태를 방지할 수 있지만, Rollback이 발생활 확률이 높고 연쇄 복귀를 초래할 수 있다고 합니다.

- 시스템 시계(system clock): 시스템 시계 값을 타임스탬프 값으로 부여합니다. 트랜잭션이 시스템에 진입할 때의 시계 값을 고유 번호로 지정하여 순서를 정하는 방법입니다.

- 논리적 계수기(counter): 트랜잭션이 발생할 때마다 카운트를 하나씩 증가시켜 타임스탬프 값으로 부여하는 방법입니다.

<br>

<strong>3. 낙관적 검증</strong>

트랜잭션 수행 중에는 어떠한 검증도 수행하지 않고, 트랜잭션 종료 시에 일괄적으로 검사하여 데이터베이스에 반영하는 기법입니다. 트랜잭션 수행 동안에는 그 트랜잭션을 위해 유지되는 데이터 항목들의 <strong>지역 사본</strong>에 대해서만 갱신이 이루어지고, 트랜잭션 종료 시에 동시성을 위한 트랜잭션 직렬화가 검증되면 일시에 DB로 반영합니다.

<br>

<center>
<img alt="concurrency-control-validation" src="https://user-images.githubusercontent.com/62047302/132208970-6a060580-7dd2-4c5a-91ae-032f7fdcb9cb.png">
</center>

<br>

낙관적 검증의 장점으로는, 교착 상태가 없고(no deadlock), 연쇄복귀 또한 없습니다(no cascading rollback).
하지만 장기 트랜잭션을 철회해야할 때에 자원을 낭비할 수 있다는 단점이 있습니다.

<br>

## 한 눈에 보기

락킹, 타임스탬프, 낙관적 검증 기법을 요약하자면 다음과 같습니다.

<center>
<img alt="concurrency-control-allinone" src="https://user-images.githubusercontent.com/62047302/132209872-be84d803-2096-4f48-9a0d-9fa94aba16d4.png">
</center>

<br>
<br>

참고: <a href="hhttps://medium.com/pocs/%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%A0%9C%EC%96%B4-%EA%B8%B0%EB%B2%95-%EC%9E%A0%EA%B8%88-locking-%EA%B8%B0%EB%B2%95-319bd0e6a68a">동시성 제어 기법 — 잠금(Locking) 기법</a>,<br> <a href="http://blog.skby.net/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%A0%9C%EC%96%B4/">데이터베이스 동시성 제어</a>,<br> <a href="http://www.jidum.com/jidums/view.do?jidumId=282">동시성제어개요</a>
