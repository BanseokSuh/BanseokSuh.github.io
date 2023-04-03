---
title: Mac OS에서 Virtualbox 안에 Ubuntu에 MySQL Docker Container 띄우고 연결
author: Banny
date: 2023-03-18 00:06:00 +0900
categories: [Server, Ubuntu]
tags: [Ubuntu, MySQL]
---

## Virtualbox 설치

<strong>\_1.</strong> Homebrew를 이용하여 Virtualbox 설치

```
brew install --cask virtualbox
```

<br>

## Virtualbox에 Ubuntu 설치

<strong>\_1.</strong> Ubuntu 공식 홈페이지를 통한 OS 이미지 다운로드

- <a href="https://ubuntu.com/download/desktop/thank-you?version=22.04.2&architecture=amd64" target="_blank">공식 홈페이지</a>

<strong>\_2.</strong> Ubuntu 이미지를 Virtualbox에 올림 <br>

- 새 OS 생성 시 다운받아놓은 Ubuntu 이미지 불러옴 <br>

    <center>
    <img alt="Ubuntu" src="https://user-images.githubusercontent.com/62047302/226078904-e4c96264-bdbc-414e-88ff-7f45250b6f69.png">
    </center>

- OS 계정의 아이디와 비밀번호 설정 <br>

    <center>
    <img alt="Ubuntu" src="https://user-images.githubusercontent.com/62047302/226078892-e8e31e72-27c4-489f-9c42-3082995b8d2f.png">
    </center>

- OS의 메모리와 CPU 할당 <br>

    <center>
    <img alt="Ubuntu" src="https://user-images.githubusercontent.com/62047302/226078898-669869fa-9c58-476c-8413-74be8e11cf3d.png">
    </center>

- 하드디스크 용량 할당 <br>

    <center>
    <img alt="Ubuntu" src="https://user-images.githubusercontent.com/62047302/226078862-938a768d-c0e1-4e9a-8795-2d41237b0f9b.png">
    </center>

- 설정한 값대로 OS 생성 <br>

    <center>
    <img alt="Ubuntu" src="https://user-images.githubusercontent.com/62047302/226078882-7d4b5746-ce3f-4b87-a445-6e9f87c964ab.png">
    </center>

<br>

<strong>\_3.</strong> Terminal이 열리지 않을 때

- 언어 설정을 바꿔줌<br>
- English(United States) -> English(United Kingdom)으로 바꿔줌<br>
- 왜? <br>

    <center>
    <img alt="Ubuntu" src="https://user-images.githubusercontent.com/62047302/226081324-7d4805a5-8f1e-4f1b-9e2a-bc627661a519.png">
    </center>

<br>

## Ubuntu 서버에 Docker 설치

<strong>\_1.</strong> 공식문서 참고

- <a href="https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository" target="_blank">공식 홈페이지</a>

<strong>\_2.</strong> apt 패키지를 업데이트하고, apt 패키지가 https로 repository를 사용할 수 있도록 패키지 다운로드

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

<strong>\_3.</strong> Docker 공식 GPG key를 다운로드 (보충 필요)

```
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

<strong>\_4.</strong> 아래 명령어로 repository를 다운로드 (보충 필요)

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

<strong>\_5.</strong> Docker 엔진을 내려받기 전 apt 패키지를 업데이트

```
sudo apt-get update
```

<strong>\_6.</strong> Docker 엔진 등 필요 플러그인 설치

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

<strong>\_7.</strong> 아래 명령어로 docker가 정상적으로 설치되었는지 확인

```
sudo docker run hello-world
```

<center>
    <img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/226161289-c01cbd9f-3556-4b7f-9cd4-7be7f1340c2b.png">
</center>

<br>

## Mysql Container 띄우기

<strong>\_1.</strong> Dockerhub에서 Mysql Image 내려받기

```
docker pull mysql:latest
```

<center>
    <img alt="mysql image" src="https://user-images.githubusercontent.com/62047302/229434556-776b264f-e5f6-4118-ae7f-d7927c1dfa70.png">
</center>

<br>

<strong>\_2.</strong> 내려받은 Mysql Image로 Container 생성 및 실행

```
docker run --name mysql -d \ // 컨테이너를 백그라운드에서 실행
  -p 3306:3306 \ // host의 3306 포트와 컨테이너의 3306 포트를 매핑
  -e MYSQL_ROOT_PASSWORD=<password> \ // root 사용자의 비밀번호를 지정
  --restart unless-stopped \ // 컨테이너가 비정상적으로 종료될 경우 재시작
  mysql:latest // 최신버전의 이미지를 사용하여 컨테이서 생성
```

<br>

<strong>\_3.</strong> Host OS와 VM 네트워크 연결 - Port Fowarding 처리

- VM의 IP를 조회하면 10.0.2.15가 나옴.

    <center>
        <img alt="vm ifconfig" src="https://user-images.githubusercontent.com/62047302/229455160-d82baa42-ddfc-4c04-b7e1-ff6c30a6c601.png">
    </center>

- Host IP, Host Port 번호로 네트워크 요청이 들어올 경우, VM의 IP, Port로 포워딩을 해줌.

    <center>
        <img alt="mysql image" src="https://user-images.githubusercontent.com/62047302/229454491-108ca308-938b-4250-ad02-8ad5600dbd79.png">
    </center>

<br>

<strong>\_4.</strong> Host OS에서 MySQL 연결

- 연결 시 Driver properties에서 <strong>allowPublicKeyRetrieval = true</strong>로 설정. (Default는 false)
- MySQL 8.0버전 이상에서부터 생길 수 있음.

    <center>
        <img alt="mysql image" src="https://user-images.githubusercontent.com/62047302/229457641-a380ccc6-bd2f-403d-a367-f1aa92d00cbe.png">
    </center>

- 정상 연결 확인

    <center>
        <img alt="mysql image" src="https://user-images.githubusercontent.com/62047302/229458785-fe3673eb-7d92-4830-9123-958c1a0ebd4b.png">
    </center>

<br>

<strong>=> MySQL Local Database 셋팅이 완료됨.</strong>

<br>
<br>
참고자료<br>
- <a href="https://stele-miha.medium.com/virtualbox-connection-from-host-to-vm-through-nat-and-natnetwork-aa15289bfc01" target="_blank">VirtualBox connection from host to VM through Nat and NatNetwork</a><br>
- <a href="https://poiemaweb.com/docker-mysql" target="_blank">14.4 Docker를 사용하여 MySQL 설치하고 접속하기</a><br>
