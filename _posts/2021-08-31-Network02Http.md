---
title: 『그림으로 배우는 Http&Network Basic』 - 2. 간단한 프로토콜 HTTP
author: Banny
date: 2021-08-31 10:30:00 +0900
categories: [Network, 『그림으로 배우는 Http&Network Basic』]
tags: [Network, 『그림으로 배우는 Http&Network Basic』]
---

## HTTP Request Messgae

<center>
<img alt="Http request" src="https://user-images.githubusercontent.com/62047302/131335403-2b11f8dc-c83b-411e-94a4-eb00decd97b8.png">
</center>

<br>

## HTTP Response Message

<center>
<img alt="Http response" src="https://user-images.githubusercontent.com/62047302/131335487-6cfc7593-76bd-4259-a08a-a077e462be0d.png">
</center>

<br>

## HTTP 프로토콜

HTTP 프로토콜은 상태를 유지하지 않습니다. 즉, 이전에 보냈던 리퀘스트나 이미 되돌려준 리스폰스에 대해서는 전혀 기억하지 않습니다.
하지만 웹이 진화함에 따라 상태를 유지할 필요가 생겼습니다. 로그인 상태가 대표적인 예입니다.
상태를 유지하기 위해서 쿠키(Cookie)라는 기술이 도입되었습니다.

<br>

## HTTP 메소드

<strong>GET</strong>: 리소스 획득<br>
<strong>POST</strong>: 엔티티 전송<br>
<strong>PUT</strong>: 파일 전송 - PUT 자체에는 인증 기능이 없어 누구든지 파일을 업로드 가능하다는 보안 상의 문제가 있어 일반 웹 사이트에서는 사용되지 않습니다.<br>
<strong>HEAD</strong>: 메시지 헤더 취득 - GET과 같은 기능이지만 메시지 바디는 돌려주지 않습니다. URI 유효성과 리소스 갱신 시간을 확인하는 목적으로 사용됩니다.<br>
<strong>DELETE</strong>: 파일 삭제 - PUT과 반대로 동작합니다.<br>
<strong>OPTION</strong>: 제공하고 있는 메소드의 문의 - 리소스가 제공하고 있는 메소드를 조사하기 위해 보냅니다. 예를 들어 preflight 요청일때 사용됩니다.<br>

<br>

## 지속연결

HTTP 초기 버전에는 HTTP 통신을 한 번 할 대마다 TCP데 의해 연결과 종료를 할 필요가 있었습니다.

하지만 HTTP가 널리 보급되면서 다량의 문서를 요청해야 할 경우들이 생겼습니다.

예를 들어, 하나의 HTML에 여러 이미지가 포함돼있는 경우, 요청을 보낼 시에 이미지를 획득하기 위해 여러 요청을 송신해야 했습니다. 매 요청마다 TCP 연결과 종료를 해야했고 결국 불필요한 통신량이 늘었습니다.

이러한 문제를 해결하기 위해 지속 연결(Persistent Connections)이라는 방법이 고안되었습니다. 어느 한 쪽이 명시적으로 연결을 종료하지 않는 이상 TCP 연결은 유지되는 것입니다.
단 1회의 TCP 연결로 리퀘스트와 리스폰스 교환을 여러 번 하게 됐습니다.

그 이점으로는, TCP 연결과 종료가 반복됨에 따른 오버헤드를 줄여주기 때문에 서버에 대한 부하가 경감됩니다.
또한 요청과 응답이 빠르게 완료되기 때문에 웹 페이지를 빨리 표시할 수도 있습니다.

<br>

<strong>파이프라인화</strong>

<center>
<img alt="http pipelining" src="https://user-images.githubusercontent.com/62047302/131347776-c8819e48-6e6d-483c-9a66-2e8a2c4a0827.png">
</center>

<br>

지속 연결은 여러 리퀘스트를 보낼 수 있도록 파이프라인(HTTP pipelining)화를 가능하게 합니다. 이로 인해, 리스폰스를 수신할 때까지 기다리지 않고 다음 리퀘스트를 보낼 수 있게 됐습니다.

<br>

<strong>쿠키</strong>

HTTP는 기본적으로 stateless한 프로토콜입니다. 상태를 유지하지 않는다는 점에서 서버의 CPU나 메모리 같은 리소스의 소비를 억제할 수 있다는 장점이 있지만, 상태를 유지해야 할 필요가 있게 됐습니다. 사용자 인증과 같은 문제일 경우에 그렇습니다.

stateless 프로토콜이라는 특징은 남겨둔 채, 이와 같은 문제를 해결하기 위해 쿠키라는 시스템이 도입되었습니다.
쿠키는 리퀘스트와 리스폰스에 쿠키 정보를 추가해서 클라이언트의 상태를 파악하기 위한 시스템입니다.

쿠키는 서버에서 리스폰스로 보내진 Set-Cookie라는 헤더 필드에 의해 쿠키를 클라이언트에 보존하게 됩니다. 다음 번에 클라이언트가 같은 서버로 리퀘스트를 보낼 때, 자동으로 쿠키 값을 넣어서 송신합니다.

<br>

- 쿠키 없는 리퀘스트

```
GET /reader/ HTTP  /1.1
Host: www.banny.com
*헤더 필드에 쿠키는 없음
```

- 서버가 쿠키 발행한 리스폰스

```
HTTP /1.1 200 OK
Date: Tue, 31 Aug 2021 07:12:20 GMT
Server: Apache
<Set-Cookie: sid=1234567890182736; path=/;expires=Wed, => 10-Oct-12 07:12:20 GMT>
Content-Type: text/plain; charset=UTF-8
```

- 쿠키 있는 리퀘스트(자동 송신)

```
GET /reader/ HTTP  /1.1
Host: www.banny.com
Cookie: sid=1234567890182736
```

<br>
<br>
참고: <a href="http://www.yes24.com/Product/Goods/15894097">『그림으로 배우는 Http&Network Basic』 - 우에노 센</a>
