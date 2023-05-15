---
title: Git 명령어
author: Banny
date: 2023-05-15 11:56:00 +0900
categories: [Git]
tags: [GitCommand]
---


## Git 명령어 정리

<strong>git status</strong>

- 현재의 git 상태 확인

<br>

<strong>git add 파일이름</strong>

- 파일을 staging area에 올림
- git add . -> 현재 경로에 있는 모든 파일을 staging area에 올림

<br>

<strong>git reset</strong>

- 파일을 staging area에서 내림

<br>

<strong>git commit -m "커밋메시지"</strong>

- staging area에 올라가 있는 파일을 커밋함

<br>

<strong>git push</strong>

- 커밋된 내역을 원격 저장소에 올림

<br>

<strong>git checkout -b 브랜치이름</strong>

- 신규 브랜치 생성

<br>

<strong>git push --set-upstream origin 브랜치이름</strong>

- 로컬 브랜치가 원격 저장소에 없을 경우에 git push 명령어는 에러를 리턴함. 원격에 해당 브랜치가 없기 때문.
- 위 명령어로 원격에 브랜치를 생성하여 push
- git push -u origin 브랜치이름 -> 조금 더 간소화된 명령어

<br>

<strong>git merge 브랜치A</strong>

- 브랜치A가 현재 브랜치로 병합됨.

<br>

<strong>git reset --hard 커밋hash</strong>

- 커밋hash의 소스코드로 rollback됨

<br>

<strong>git rebase main</strong>

- main 브랜치에서 신규 생성한 A 브랜치에서 작업 중일 때, 다른 작업자가 main 브랜치의 소스코드를 수정하여 push를 하게 되는 경우가 있음<br>

    <center>
    <img alt="git" src="https://github.com/BanseokSuh/BanseokSuh.github.io/assets/62047302/f49247b5-8dfc-4209-89ac-1e7e5878d595">
    </center>

- git merge를 하게 되면 아래와 같이 커밋 내역이 생성됨<br>

    <center>
    <img alt="git" src="https://github.com/BanseokSuh/BanseokSuh.github.io/assets/62047302/92208a88-d418-4016-a0c1-1c54aa9c1b4a">
    </center>
    
- 브랜치가 많아지게 될 경우에 아래처럼 커밋 내역을 읽기가 어려워짐<br>

    <center>
    <img alt="git" src="https://github.com/BanseokSuh/BanseokSuh.github.io/assets/62047302/58db24d2-39ab-4609-aece-6d033285b67b">
    </center>

- git rebase는 A 브랜치의 base를 main 브랜치의 최신 커밋으로 옮김<br>

    <center>
    <img alt="git" src="https://github.com/BanseokSuh/BanseokSuh.github.io/assets/62047302/0f345ce2-cbd2-4adf-9d9c-4339bedcb33a">
    </center>

- 명령어 그대로 해당 브랜치의 베이스를 main 브랜치의 최신 커밋으로 re-base홤

- commit history가 깔끔해짐


<br>

