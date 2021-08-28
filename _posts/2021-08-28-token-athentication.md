---
title: 토큰 인증 방식
author: Banny
date: 2021-08-28 13:35:00 +0900
categories: [Network]
tags: [Network, Token Athentication]
---

## 토큰 인증방식 flow

<img src="https://user-images.githubusercontent.com/62047302/131057242-f45f9fcf-fdd0-4dad-a9b2-e58f5edfe6f3.png">

<br>

## 왜 토큰 인증 방식?

기존의 인증 시스템은 서버 기반의 인증 방식으로, 서버 측에서 사용자들의 정보를 기억하고 있어야 했습니다.
세션 인증 방식인 경우에는, 세션 스토리지에서 그런 역할을 했죠.
하지만 서버를 확장하는 경우에는 세션을 분산을 해야 하는데, 그 과정은 매우 어렵고 복잡하다고 합니다.

토큰 기반의 인증 방식은 사용자의 인증 정보를 서버나 세션에 유지하지 않고 클라이언트 측에서 돌아오는 요청만으로 작업을 처리합니다.

그렇기 때문에 인증 정보가 서버에 종속되지 않고, 어떤 서버로 요청을 보내도 상관이 없습니다.
서버가 확장한다고 하더라도 크게 문제가 될 것이 없습니다.

<br>

## AccessToken과 RefreshToken

access token을 통한 인증 방식의 문제점은 보안에 취약할 수 있다는 것입니다.

보안을 높이기 위해 access token의 유효기간을 짧게 설정할 수 있지만, 그렇게 된다면 사용자가 로그인을 자주 해서 매번 토큰을 발급받아야 하는 불편함이 있습니다.

그래서 나온 것이 refresh token입니다.
refresh token은 access token과 같은 형태의 jwt입니다.
refresh token은 access token이 만료됐을 때 그것을 새롭게 발급해주는 열쇠가 됩니다.

<br>

## 토큰 인증 방식 설계

- 사용자가 로그인하면 access token과 refresh token을 발급해줍니다.
- access token은 refresh token에 비해 유효기한이 짧습니다.
  예를 들어, access token은 1시간, refresh token은 2주의 기한을 가질 수 있습니다.
- access token은 username을 payload에 담고 있습니다.
- refresh token은 DB에 key: username, value: token의 형태로 가지고 있습니다.
- Client가 refresh token과 만료된 access token을 가지고 재발급 요청을 보내면<br>
  1.  만료된 토큰에서 username을 얻고<br>
  2.  username을 key로 해서 DB에 존재하는지 확인하고<br>
  3.  그 유효기간도 확인합니다.
- 위에서 통과를 하지 못했다면, 다시 로그인을 통해 access token + refresh token을 처음부터 발급합니다.

<br>

## 시나리오

1. 로그인을 하면 access token과 refresh token을 모두 발급합니다.
   이 때, refresh token은 서버의 DB에 저장하고, access token은 쿠키에 클라의 저장합니다.<br>
2. 사용자가 인증이 필요한 api에 접근하고자 하면, 가장 먼저 토큰을 검사하는 미들웨어를 지나야 합니다.
   이 때, 토큰을 검사함과 동시에 각 경우에 대해 토큰의 유효기간을 확인하여 재발급 여부를 결정합니다.<br>
   a. access token과 refresh token 모두가 만료된 경우 ::: 에러 발생<br>
   b. access token은 만료, refresh token은 유효한 경우 ::: access token 재발급<br>
   c. access token은 유효, refresh token은 만료된 경우 ::: refresh token 재발급<br>
   d. access token과 refresh token 모두가 유효한 경우 ::: 다음 미들웨어로<br>

3. 로크아웃을 하면 access token과 refresh token을 모두 만료시킵니다.

<br>

## 코드

> **Note**: 아래의 코드들은 본인이 직접 작성한 것이 아니라 참고 블로그를 보고 이해를 위해 타이핑했음을 밝힙니다.

<br>

<strong>라우터</strong>

```js
const user = require("../controllers/user");
const { checkTokens, checkUser } = require("../middleWares/user");
const router = express.router();

...
router.post("/login", user.login);
router.get("/read", checkTokens, checkUser, user.read);
```

<br>
<strong>유틸</strong>

```js
const jwt = require("jsonwebtoken");

...
module.exports = {
  verifyToken(token) {
    try {
      return jwt.verify(token, process.env.JWT_SECRET);
    } catch (e) {
      // if (e.name === 'TokenExpiredError') return null;
      return null;
    },
    ...
  }
}
```

<br>
<strong>시나리오1: 로그인</strong>

`/controller/user.js`

```js
const controller = {
  async login(req, res, nest) {
    /* POST 요청을 통해 req.body에 담긴 사용자의 id와 name을 추출하는 로직 */
    ...
    // refresh token의 payload에는 빈 객체. 필요가 없기 때문.
    // jwt는 payload가 늘어날수록 오버헤드가 커진다.
    const refreshToken = jwt.sign({},
      process.env.JWT_SECRET, {
        expiresIn: '14d',
        issuer: 'banny'
      }
    );
    const connection = await pool.getConnection(async conn => await conn);
    try {
      // DB에 refresh token 저장
      await connection.beginTransaction();
      await connection.query(`
        INSERT INTO
        tokens(content, userId)
        VALUE
        (?, ?);
      `, [refreshToken, userId]);
      await connection.commit();

      // 토큰 세팅
      const accessToken = jwt.sign({ userId, username },
        process.env.JWT_SECRET, {
          expiresIn: '1h',
          issuer: 'banny'
        }
      );

      // 웹 브라우저(클라)에 토큰 세팅
      res.cookie('accessToken', accessToken);
      res.cookie('refreshToken', refreshToken);
      next();
    } catch (e) {
      await connection.rollback();
      next(e);
    } finally {
      connection.release();
    }
  },
  ...
};

module.exports = controller;
```

<br>
<strong>시나리오2: token 검사 및 재발급</strong>

`/middleware/user.js`

아래의 4가지 경우에 대해서 검사를 진행합니다.

a. access token과 refresh token 모두가 만료된 경우 ::: 에러 발생<br>
b. access token은 만료, refresh token은 유효한 경우 ::: access token 재발급<br>
c. access token은 유효, refresh token은 만료된 경우 ::: refresh token 재발급<br>
d. access token과 refresh token 모두가 유효한 경우 ::: 다음 미들웨어로<br>

```js
const { verifyToken } = require("../utils/jwt");

module.exports = {
  async checkTokens(req, res, next) {
    /* access token 자체가 없는 경우에는 에러(401)를 반환
    클라 측에선 401을 응답받으면 로그인 페이지로 이동 */
    if (req.cookie.accessToken === undefined)
      throw Error("API 사용 권한이 없습니다.");

    const accessToken = verifyToken(req.cookies.accessToken);
    const refreshToken = verifyToken(
      req.cookies.refreshToken
    ); /* 실제로는 DB 조회 */

    if (accessToken === null) {
      if (refreshToken === undefined) {
        // a: access token과 refresh token 모두가 만료된 경우
        throw Error("API 사용 권한이 없습니다.");
      } else {
        // b: access token은 만료, refresh token은 유효

        /* DB를 조회해서 palyload에 담을 값들을 가져오는 로직 */
        const newAccessToken = jwt.sign(
          { userId, username },
          process.env.JWT_SECRET,
          {
            expiresIn: "1h",
            issuer: "banny",
          }
        );
        res.cookie("accessToken", newAccessToken);
        req.cookies.accessToken = newAccessToken;
        next();
      }
    } else {
      if (refreshToken === undefined) {
        // c: access token은 유효, refresh token은 만료
        const newRefreshToken = jwt.sign({}, process.env.JWT_SECRET, {
          expiresIn: "14d",
          issuer: "banny",
        });

        /* DB에 새로 발금된 refresh token 삽입 */
        res.cookie("refreshToken", newRefreshToken);
        req.cookies.refreshToken = newRefreshToken;
        next();
      } else {
        // d: access token과 refresh token 모두가 유효한 경우
        next();
      }
    }
  },
  ...
};
...
```

<br>
<br>
참고: <a href="https://cotak.tistory.com/102">https://cotak.tistory.com/102</a>
