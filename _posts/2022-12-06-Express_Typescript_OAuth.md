---
title: Typescript 기반의 Express 서버에서 카카오 OAuth 연동
author: Banny
date: 2022-12-06 23:37:00 +0900
categories: [Network, Authentication]
tags: [Kakao OAuth]
---

## Kakao Oauth

1. 카카오 DEV 콘솔에서 CLIENT_ID, CLIENT_SECRET을 발급받고, 인가 코드를 받을 REDIRECT_URI을 설정한다.
2. 카카오 서버로부터 인가 코드를 받는다.
3. 인가 코드, CLIENT_ID, CLIENT_SECRET으로 카카오 서버로부터 ACCESS_TOKEN을 발급받는다.
4. ACCESS_TOKEN으로 카카오 서버부터 사용자의 정보를 받는다.

<br>

## 프로젝트 구조

<center>
<img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941596-477975c0-2b7a-42a1-9d2d-1fb465f78026.png">
</center>

<br>

### 1. 카카오 DEV 콘솔에서 key 발급

a. **CLIENT_ID** 발급 - REST API key

   <center>
   <img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941595-d07df4c2-1a21-41ff-b900-5c7a0dd6401e.png">
   </center>

b. **CLIENT_SECRET** 발급

   <center>
   <img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941590-bd854721-27e0-44f3-8026-f2cff27dcf2d.png">
   </center>

c. **REDIRECT_URI** 설정

- 인가 코드를 전달할 api 서버의 주소

   <center>
   <img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941589-a6a32dbf-2ea6-4186-9aa5-e61063065557.png">
   </center>

<br>

### 2. 카카오 로그인 진입점 설정

```js
<a href="https://kauth.kakao.com/oauth/authorize?response_type=code&client_id={Client_ID}&redirect_uri={Redirect_URI}">
  카카오 로그인
</a>
```

   <center>
   <img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941588-ecbc866d-0091-437d-a7d3-d853cd505d47.png">
   </center>

a. 카카오 로그인 페이지로 이동<br>
b. CLIENT_ID, REDIRECT_URI 필요

<center>
<img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205944209-aef7d56e-12e5-4ad5-9f33-9dbe6371e899.png">
</center>

<br>

### 3. 로그인 후 카카오 서버로부터 **인가 코드**를 받고, 카카오 DEV 콘솔에서 설정했던 api 서버의 redirect uri로 요청

<center>
<img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941584-f48daf12-186a-43d1-9267-1be752c1969d.png">
</center>

<br>

### 4. 발급받은 코드와 CLIENT_ID, CLIENT_SECRET을 갖고 카카오 서버로 **ACCESS_TOKEN 요청**

<center>
<img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941572-22f84e9e-d106-4ec1-91a7-2010749eb07c.png">
</center>

<br>

### 5. 발급받은 access token을 갖고 카카오 서버로 사용자 정보 요청

<center>
<img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941568-9124379f-0337-4d75-9255-ead10323598c.png">
</center>

<br>

### 6. 유저 정보 획득

<center>
<img alt="kakao oauth" src="https://user-images.githubusercontent.com/62047302/205941550-f588a6b4-be87-4f51-aa0c-b1c0d3f923e2.png">
</center>

- 어떤 정보를 가져올지는 카카오 DEV 콘솔에서 설정 가능

⇒ 해당 데이터로 **회원가입** 처리, **로그인** 처리, **access-token 발급 및 캐싱**

<br>

## 참고자료

https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api

<br>
