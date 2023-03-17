---
title: Ubuntu 서버에서 MySQL 완전 삭제
author: Banny
date: 2023-03-11 00:06:00 +0900
categories: [Server, Ubuntu]
tags: [Ubuntu]
---

## Ubuntu 서버에서 MySQL을 CLI로 완전 삭제

<strong>1\_삭제</strong>

```
sudo apt-get remove --purge mysql*
```

sudo: OS의 관리자 권한으로 실행하는 명령어<br>
apt-get: Advanced Packaging Tool의 약어로, 우분투를 포함한 데비안 계열의 리눅스에서 쓰이는 패키지 관리 도구 명령어<br>
remove: 패키지를 삭제. 하지만 설정 파일은 남겨둠.<br>
--purge: 해당 옵션을 추가하면 설정 파일도 함께 삭제함.<br>
mysql\*: 'mysql'로 시작하는 모든 패키지 삭제

<br>
<strong>2_mysql 관련 파일 목록 확인</strong>

```
dpkg -l | grep mysql
```

dpkg -l: 데비안 패키지 관리 시스템으로 설치된 패키지 목록 조회<br>
| grep mysql: 조회된 목록 중에서 'mysql'문자열에 해당하는 패키지 검색

<br>
<strong>3_폴더 및 관련항목 삭제</strong>

```
sudo rm -rf /etc/mysql /var/lib/mysql
sudo rm -rf /var/log/mysql
sudo rm -rf /var/log/mysql.*
sudo rm /var/lib/dpkg/info/*
sudo apt-get autoremove
sudo apt-get autoclean
```

autoremove: 삭제한 패키지에 의존성이 있어서 더이상 필요없는 패키지도 함께 삭제<br>
autoclean: 불완전하게 다운로드된 패키지 삭제

<br>

// wow
