---
title: 『그림으로 배우는 Http&Network Basic』 - 4. 결과를 전달하는 HTTP 상태 코드
author: Banny
date: 2021-08-31 17:00:00 +0900
categories: [Network, 『그림으로 배우는 Http&Network Basic』]
tags: [Network, 『그림으로 배우는 Http&Network Basic』]
---

## 상태 코드

상태코드는 간략하게 다음과 같습니다.

<center>
<img alt="http status code2" src="https://user-images.githubusercontent.com/62047302/131454720-1600f497-a703-4f3f-887d-26a72a2aaf5c.png">
</center>

<br>

## 2xx 성공 (Success)

리퀘스트가 정상적으로 처리됐음을 나타냅니다.

- 200 OK: 클라이언트의 리퀘스트를 서버가 정상 처리했을을 나타냅니다.
- 204 No Content: 서버가 리퀘스트를 처리하는 데는 성공했지만, 리스폰스 엔티티 바디를 포함하지 않습니다.
- 206 Partial Content: Range에 의해서 범위가 지정된 리퀘스트에 의해 서버가 부분적으로 GET 리퀘스트를 받았음을 나타냅니다. 리스폰스에는 Content-Range로 지정된 범위의 엔티티가 폼함됩니다.

<br>

## 3xx 리다이렉트 (Redirection)

리퀘스트의 정상적 처리 종료를 위해 브라우저 측에서 특별한 처리를 수행해야 함을 나타냅니다.

- 301 Moved Permanently
- 302 Found
- 303 See Other
- 304 Not Modified
- 307 Temporary Redirect

<br>

## 4xx 클라이언트 에러 (Client Error)

클라이언트의 원인으로 에러가 발생했음을 나타냅니다.

- 400 Bad Request: 리퀘스트 구문이 잘못되었음을 나타냅니다. 브라우저는 이것을 200 OK와 같이 취급합니다.
- 401 Unauthorized: 리퀘스트에 HTTP 인증 정보가 필요하다는 것을 나타냅니다.
- 403 Forbidden: 리퀘스트된 리소스의 액세스가 거부되었음을 나타냅니다. 그 이유를 명확하게 하는 경우에는 리스폰스 엔티티 바디에 기재해서 유저 측에 표시합니다.
- 404 Not Found: 리퀘스트한 리소스가 서버상에 없다는 것을 나타냅니다.

<br>

## 5xx 서버 에러 (Server Error)

서버 원인으로 에러가 발생하고 있을을 나타냅니다.

- 500 Internal Server Error: 서버에서 리퀘스트를 처리하는 도중에 에러가 발생했음을 나타냅니다.
- 503 Service Unavailable: 일시적으로 서버가 과부하 상태이거나 점검중이기 때문에 현재 리퀘스트를 처리할 수 없음을 나타냅니다.

<br>
<br>
참고: <a href="http://www.yes24.com/Product/Goods/15894097">『그림으로 배우는 Http&Network Basic』 - 우에노 센</a>
