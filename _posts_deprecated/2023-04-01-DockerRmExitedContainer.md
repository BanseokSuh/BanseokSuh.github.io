---
title: Docker Exited된 container 삭제하기
author: Banny
date: 2023-04-01 18:43:00 +0900
categories: [Server, Docker]
tags: [Docker]
---

## Container 상태 확인

- docker ps : Up 상태의 컨테이너 목록과 상태 조회
- docker ps -a : 모든 컨테이너 목록과 상태 조회
- 하단 이미지에서의 명령어로 조회 결과를 포맷팅할 수 있음.

<center>
<img alt="Container" src="https://user-images.githubusercontent.com/62047302/229393053-0e73f0bc-add5-4423-8e63-743491bd82d2.png">
</center>

<br>

## Exited 상태의 컨테이너 일괄 삭제

- docker ps -aq : 모든 컨테이너에 대해 이름만 조회
- 하단 이미지로 컨테이너 삭제 시 현재 Up 상태의 컨테이너는 실행 중인 상태라 삭제되지 않음.

<center>
<img alt="Container" src="https://user-images.githubusercontent.com/62047302/229392884-ddf7e502-adc4-47ae-b571-d9c14d814fd3.png">
</center>

<br>

## Container 확인

- Exited된 컨테이너들 삭제 확인

<center>
<img alt="Container" src="https://user-images.githubusercontent.com/62047302/229393139-8e02bc90-e7ad-41ef-8f0f-e0919bcaf037.png">
</center>

<br>
