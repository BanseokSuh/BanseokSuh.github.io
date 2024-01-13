---
title: 실시간 알림 서비스 개발기
author: Banseok Suh
date: 2024-01-11 22:35:00 +0900
categories: [Project]
tags: [Socket.io, Redis, Pub/Sub]
---

# 실시간 알림 서비스

<br>

Http 통신 기반의 RESTful API를 주로 개발해왔던 평소와는 다르게,<br>
이번에는 Socket 통신을 하는 실시간 알림 피쳐를 개발하게 되었다.

여러 글을 참고해 아래와 같이 설계하게 되었다.

<br>

# 설계

1. Web이 Socket 서버에 connect
2. Socket 서버는 Redis 채널 Subscribe
3. Api 서버는 이벤트 발생 시 Redis 채널에 Publish
<!-- https://github.com/BanseokSuh/blog/assets/62047302/02fea7b7-c999-4b29-a2c0-7069f103175c -->

# Api Server

- Publish

# Socket.io

- Subscribe

<br>
<br>
참고자료<br>
<!-- - <a href="https://cotak.tistory.com/102">https://cotak.tistory.com/102</a> -->
