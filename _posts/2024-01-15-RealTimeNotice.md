---
title: 실시간 알림 서비스 개발기
author: Banseok Suh
date: 2024-01-13 22:35:00 +0900
categories: [Project]
tags: [Socket.io, Redis, Pub/Sub]
---

# 실시간 알림 서비스

회사에서 개발 중인 LMS(교육관리시스템)에 새로운 피쳐가 기획되었다. 로그인한 교육자가 학생들의 교육 현황을 실시간 알림으로 받는 피쳐이다.
<br>
Stateless한 http 통신으로는 실시간 알림을 받을 수 없다. 화면에 새로 진입하거나 새로고침을 할 경우 api 요청을 통해 알림 데이터를 조회할 수 있지만, 실시간으로 전송되지는 않는다.
<br>
실시간 통신에는 Polling, SSE(Server Sent Event), WebSocket과 같은 여러 종류의 통신이 사용되는데,
<br>
그 중 **WebSocket**을 사용하기로 결정했다.
<br>
<br>
이유는,
<br>

1. 이미 socket.io로 작동하고 있는 socket 서버가 존재했다.
   <br>
2. 서버에서 클라이언트로 단방향으로 보내주기만 하는 기능이라 SSE를 사용해도 됐지만, 추후 교육자와 학생 간의 채팅과 같은 양방향 통신를 고려했을 때 WebSocket으로 구현하는 것이 더 **확장성**이 있을 것 같았다.

<br>
<br>
좋아, 이제 구조를 잡아보자.
<br>
고민하고 여러 자료를 참고하면서 아래와 같은 구조로 설계를 했다.
<br>
<br>

# 설계

1. Web이 Socket 서버에 connect
2. Socket 서버는 Redis 채널 Subscribe
3. Api 서버는 이벤트 발생 시 Redis 채널에 Publish
4. Redis는 Publish된 데이터를 Socket으로 보내고, Socket에서는 데이터 가공하여 Web에 반환

<center>
<img alt="realtimeNoticeArchitecturev2" src="https://github.com/BanseokSuh/BanseokSuh.github.io/assets/62047302/5f0f0f09-da29-45f6-8433-825a3acff7b0">
</center>

- 원래는 알림 이력을 MariaDB에 쌓으려 했으나 json의 특정 필드가 조회 조건으로 사용되도록 기획이 변경되었음..
- 유연한 스키마를 가진 MongoDB를 알림 이력 테이블로 변경하기로 결정했다.

<br>

# Web

- Web에는 연결 정보를 공유했다.

  - **Namespace : `/알림`**
  - **Listener : `get_알림`**

# Api Server

- 학생 유저가 특정 이벤트를 발생하면,

  - MongoDB 알림\_이력 컬렉션에 신규 doc 생성된다.

- 그리고, 해당 학생의 교육자 계정에 해당하는 Redis 채널로 알림 데이터를 **publish**

# Socket.io

- 교육자가 로그인한 상태에서 Web은 네임스페이스, 리스너로 Socket.io와 연결되어 있고, 연결 시 socket 서버에 access token을 전달한다.
- Socket 서버에서는 access token에서 userIndex 추출한다.
- 추출한 userIndex를 사용하여 subscribe할 channel 생성 후 구독한다.
- **subscribe**를 통해 조회된 데이터로 MariaDB에서 데이터 조회 후 가공한다.

  - 반환 데이터

        {
            "알림_고유_아이디": Number,
            "교육자_유저인덱스": Number,
            "학생_유저인덱스": Number,
            "학생_유저아이디": String,
            "학생_유저이름": String,
            "열람일": Date,
            ...
        }

<br>

# 설계 변경

## 알림 이력 적재 Database 변경 (MariaDB -> MongoDB)

- MariaDB의 알림 이력 테이블 내 json 문자열이 적재되는 TEXT 속성의 컬럼이 있었다.
- 해당 json의 각 필드들은 **join문의 조건**으로 사용되고, **Driven 테이블의 join 조건**으로 사용될 것이어서 인덱싱이 필요가 없었다. (Driving 테이블의 join 조건 컬럼에 이미 인덱싱이 되어 있었음)
- 하지만 json의 특정 필드가 **조회 조건**으로 사용되도록 기획이 변경되었다..
- json의 특정 필드를 가상 컬럼으로 만들고 해당 컬럼을 인덱싱하는 방법도 존재했다.
- 하지만 추후 다른 타입의 알림 데이터를 적재 및 조회해야 할 경우 해당 테이블에 어떤 데이터가 들어갈지도 모르고, 그 때마다 가상컬럼을 만들어 인덱싱하는 것도 효율적인 방법은 아닌 것 같았다.
- 관계형 데이터베이스보다 더 유연한 스키마를 가진 MongoDB를 알림 이력을 저장하는 데이터베이스로 변경하고, 객체 안의 필드를 인덱싱해서 조회하는 것으로 결정했다.
- 그럼 가상 컬럼 만들 필요도 없고, 다른 형식의 json이 저장되고 조회 조건으로 사용될 시, 해당 필드를 그냥 인덱싱해주면 된다.

<br>

# Trouble shooting

## 간헐적 알림 발송 안 되는 이슈

### Trouble

- 알림과 관련된 모든 기능을 완료하고 테스트용 api를 만들어 알림 발송 테스트를 진행했다.
- 알림을 1개씩 발송했을 때, 보통은 정상적으로 발송이 되었다.
- 하지만 **간헐적으로 하나씩 발송이 안 되는 이슈**가 있었다.
- 정확한 테스트를 위해 알림을 여러 개 보내는 테스트 api를 만들어 10개, 20개씩 알림을 보내 테스트를 진행했다.
- 그 중에 1-2개, 많으면 3개 정도의 알림이 정상적으로 보내지지 않았다.
- 에러 로그를 보니,
  - 알림을 전송할 때 알림 발송자는 api를 통해 MongoDB에 이력을 하나 쌓고,
  - 알림 수신자는 MongoDB에서 해당 알림 이력을 읽어서 데이터 가공 후 수신하게 되는데,
  - 알림 수신자가 해당 이력을 찾지 못했다는 에러가 발생한 것이다.
  - MongoDB를 직접 조회해보니 데이터는 분명히 있었다.

### Shooting

- 데이터는 존재하는데, 해당 id로 조회를 못한다니?
- 이해가 안 되는 상황이었지만, **Replica-set**의 경우라면 그럴 수도 있을 거란 생각이 들었다.
- Replica-set으로 연결되어 있는 MongoDB의 경우, 데이터 저장을 Primary에서, 조회를 Secondary에서 한다.
- 데이터는 존재하는데 곧바로 조회가 되지 않는 이유는, **Primary 노드에서 Secondary 노드로 복제가 되기 전에 Secondary 노드로 조회 쿼리**를 보냈기 때문이라고 생각했다.
- 그렇지 않고서는 해당 에러가 발생할 일이 거의 없을 것 같았다.
- 이슈 해결을 위해 조회 쿼리를 수정했다.
- 데이터를 읽을 때 Primary 노드에서 읽도록 옵션을 추가해주면 된다.
- 쿼리 수정 후 이슈 완전 해결되었다.

  ```js
  // as-is
  const 알림이력 = await 알림컬렉션.findOne({ id: 알림이력id }).lean();
  ```

  ```js
  // to-be
  const 알림이력 = await 알림컬렉션
      .findOne({ id: 알림이력id },
      .read('primaryPreferred') // primary 노드에서 조회하도록 하는 옵션
      .lean();
  );
  ```

<br>

## Socket.io custom header 못 보내는 이슈

### Trouble

- Socket 서버에서 언어 코드값('ko', 'en' 등)이 필요했기 때문에 extraHeaders로 언어값 전송 헤더 붙여서 통신을 시도했다.
- 하지만 transports: ["websocket"] 통신일 경우에는 extraHeaders는 무시된다고 한다. (polling 통신일 경우에만 extraHeaders 가능)
  - 브라우저의 websocket api가 custom header를 허용하지 않기 때문이다.
  - "this will not work when using only WebSocket in a browser"
    - <a href="https://socket.io/docs/v3/client-initialization/#extraheaders" target="_blank">Socket.io Extraheaders 링크</a>

### Shooting

- handshake 요청 시 header가 아니라 query param으로 content-language 값을 보내서 언어 코드값 전달 받음

  ```js
    const {
        ...
        handshake: {
            ...
            query: { 'content-language': langCd }
        },
        ...
    } = socket;
  ```

  <br>

## Socket 연결이 중간에 끊기는 이슈

### Trouble

- 교육자 계정으로 로그인 후 인터렉션 없을 경우 연결이 끊어져 알림 발송되지 않은 이슈를 발견했다.

### Shooting

- Web에서 socket 서버 연결 시 옵션으로 하단 옵션 추가해서 연결 요청하여 자동 재연결을 활성화했다.

  ```js
  {
      "reconnection": true
  }
  ```

<br>
<br>

<br>
<br>
참고자료<br>
- <a href="https://socket.io/docs/v3/client-initialization/#extraheaders">https://socket.io/docs/v3/client-initialization/#extraheaders</a>
