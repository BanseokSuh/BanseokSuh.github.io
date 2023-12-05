---
title: Command-line에서 Intellij 실행시키기
author: Banny
date: 2023-04-03 23:43:00 +0900
categories: [Tools, Intellij]
tags: [Intellij]
---

## Intellij > Tools > Create Command-line Launcher

- Intellij에서 Create Command-line Launcher를 실행한다.

<center>
<img alt="Container" src="https://user-images.githubusercontent.com/62047302/235448388-9d243989-64fc-4de1-80dc-c9eb82acfeff.png">
</center>

<br>

- 타 블로그들에 의하면 Intellij를 여는 스크립트를 저장할 위치를 설정하는 창이 정상적으로 나온다.
- 하지만 나의 경우 아래와 같은 메시지만 나왔다.
- 해당 메시지는 $PATH 환경변수에 Intellij 실행 파일 경로를 추가하라는 메시지이다.

<center>
<img alt="Container" src="https://user-images.githubusercontent.com/62047302/235448361-87d4b27f-2c05-43a9-a16f-50c88d2b326f.png">
</center>

<br>

## $PATH 환경변수에 실행 파일 경로 추가

- zsh을 사용하고 있기 때문에 zshrc 파일을 열어 환경변수에 경로를 추가한다.
- 해당 경로는 운영체제가 실행 파일을 찾는 경로이다.
- 즉, idea를 입력하면 운영체제가 $PATH 환경변수에 등록된 경로들 중에서 idea라는 이름을 가진 실행 파일을 찾아서 실행시킨다.

```java
// zshrc 파일 열기
nano ~/.zshrc
```

```java
// 환경변수 추가
export PATH=/Applications/IntelliJ\ IDEA\ CE.app/Contents/MacOS:$PATH
```

<br>

## Intellij 실행

- 하단 키워드로 터미널에서 Intellij를 실행시킬 수 있다.

```java
idea .
```
