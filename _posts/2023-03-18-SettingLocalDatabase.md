---
title: Mac 환경에서 virtual box 안에 ubuntu에 mysql docker container 띄우기 (작성중)
author: Banny
date: 2023-03-18 00:06:00 +0900
categories: [Server, Ubuntu]
tags: [Ubuntu, MySQL]
---

## Virtualbox 설치

<strong>\_1.</strong> homebrew를 이용하여 Virtualbox 설치

```
brew install --cask virtualbox
```

<br>

## Virtualbox에 ubuntu 설치

<strong>\_1.</strong> ubuntu 공식 홈페이지를 통해 os 이미지를 다운받는다.

- <a href="https://ubuntu.com/download/desktop/thank-you?version=22.04.2&architecture=amd64" target="_blank">공식 홈페이지</a>

<strong>\_2.</strong> ubuntu 이미지를 Virtualbox에 올린다. <br>

- 새 os 생성 시 다운받아놓은 ubuntu 이미지를 불러온다. <br>

    <center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226078904-e4c96264-bdbc-414e-88ff-7f45250b6f69.png">
    </center>

- os 계정의 아이디와 비밀번호를 설정한다. <br>

    <center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226078892-e8e31e72-27c4-489f-9c42-3082995b8d2f.png">
    </center>

- os의 메모리와 CPU를 할당한다. <br>

    <center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226078898-669869fa-9c58-476c-8413-74be8e11cf3d.png">
    </center>

- 하드디스크 용량을 할당한다. <br>

    <center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226078862-938a768d-c0e1-4e9a-8795-2d41237b0f9b.png">
    </center>

- 설정한 값대로 os를 생성한다. <br>

    <center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226078882-7d4b5746-ce3f-4b87-a445-6e9f87c964ab.png">
    </center>

<br>

<strong>\_3.</strong> terminal이 열리지 않을 때

- 언어 설정을 바꿔준다.<br>
- English(United States) -> English(United Kingdom)으로 바꿔주었다.<br>
- 왜? <br>

    <center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226081324-7d4805a5-8f1e-4f1b-9e2a-bc627661a519.png">
    </center>

<br>

## ubuntu 서버에 docker 설치

<strong>\_1.</strong> 공식문서 참고

- <a href="https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository" target="_blank">공식 홈페이지</a>

<strong>\_2.</strong> apt 패키지를 업데이트하고, apt 패키지가 https로 repository를 사용할 수 있도록 패키지를 내려받는다.

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

<strong>\_3.</strong> Docker 공식 GPG key를 내려받는다.

```
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

<strong>\_4.</strong> 아래 명령어로 repository를 셋팅한다.

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

<strong>\_5.</strong> Docker 엔진을 내려받기 전 apt 패키지를 업데이트한다.

```
sudo apt-get update
```

<strong>\_6.</strong> Docker 엔진, containerd, Docker Compose를 설치한다.

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

<strong>\_7.</strong> 아래 명령어로 docker가 정상적으로 설치되었는지 확인한다.

```
sudo docker run hello-world
```

<center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226161289-c01cbd9f-3556-4b7f-9cd4-7be7f1340c2b.png">
</center>

<br>

## mysql container 띄우기

(작성중)
