---
title: 『그림으로 배우는 Http&Network Basic』 - 6. HTTP 헤더_01
author: Banny
date: 2021-09-05 23:15:00 +0900
categories: [Network, 『그림으로 배우는 Http&Network Basic』]
tags: [Network, 『그림으로 배우는 Http&Network Basic』]
---

## 리퀘스트 HTTP 메시지 헤더

다음은 mdn에서의 리퀘스트 헤더 예시입니다.

리퀘스트 메시지 헤더에는 메소드, URI, HTTP 버전, HTTP 헤더 필드 등으로 구성되어 있습니다.

```
GET /home.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/testpage.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1
If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
Cache-Control: max-age=0
```

<br>

## 리스폰스 HTTP 메시지 헤더

다음은 mdn에서의 리스폰스 헤더 예시입니다.

리스폰스 메시지 헤더에는 HTTP 버전, 상태 코드, HTTP 헤더 필드 등으로 구성되어 있습니다.

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding
X-Backend-Server: developer2.webapp.scl3.mozilla.com
X-Cache-Info: not cacheable; meta data too large
X-kuma-revision: 1085259
x-frame-options: DENY
```

<br>

## HTTP 헤더 필드

클라이언트와 서버간의 통신에서 부가적으로 중요한 정보를 전달하는 역할을 담당하고 있습니다.
또한 메시지 바디의 크기나, 사용하고 있는 언어, 인증 정보 등을 브라우저나 서버에 제공하기 위해 사용되고 있습니다.

기본적으로 "필드 명: 필드 값"의 구조를 갖고 있습니다.

헤더 필드는 그 용도에 따라 다음의 4가지로 분류됩니다.

- <strong>일반 헤더 필드</strong>: 리퀘스트 메시지와 리스폰스 메시지 둘 다 사용되는 헤더입니다.
- <strong>리퀘스트 헤더 필드</strong>: 리퀘스트 메시지에 사용되는 헤더로, 리퀘스트의 부가 정보, 클라 정보, 리스폰스 콘텐츠에 관한 우선 순위 등을 부가합니다.
- <strong>리스폰스 헤더 필드</strong>: 리스폰스 메시지에 사용되는 헤더로, 리스폰스의 정보, 성버 정보, 클라이언트의 추가 정보 요구 등을 부가합니다.
- <strong>엔티티 헤더 필드</strong>: 리퀘스트, 리스폰스 메시지에 포함된 엔티티에 사용되는 헤더로, 콘텐츠 갱신 시간 등의 엔티티에 관한 정보를 부가합니다.

<!-- ### <strong>일반 헤더 필드</strong>

<strong>Cache-Control</strong>

디렉티브로 불리는 명령을 사용해 캐싱 동작을 지정합니다.

사용 가능한 디렉티브를 리퀘스트와 리스폰스로 나눠서 나타냅니다.

to be continued...

### <strong>리퀘스트 헤더 필드</strong>

### <strong>리스폰스 헤더 필드</strong>

### <strong>엔티티 헤더 필드</strong>

### <strong>쿠키를 위한 헤더 필드</strong> -->

<br>
<br>
참고자료<br>
- <a href="http://www.yes24.com/Product/Goods/15894097">『그림으로 배우는 Http&Network Basic』 - 우에노 센</a>
