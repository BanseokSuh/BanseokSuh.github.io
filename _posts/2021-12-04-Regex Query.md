---
title: 『MongoDB』 regex
author: Banny
date: 2021-12-04 15:21:00 +0900
categories: [DB, MongoDB]
tags: [MongoDB]
---

# MongoDB 쿼리

- sql의 쿼리와는 다르다.
- firebase를 사용해봤는데, 그것과 거의 같은 문법이다.
- 특정 문자를 포함한 데이터를 찾고 싶다면 {$regex: '문자열'} 구문을 사용해 찾을 수 있다.
  ```js
  db.컬렉션이름
    .find({ 키: "밸류", 키: { $regex: "문자열" } })
    .projection({})
    .sort({ _id: -1 })
    .count();
  ```

# ipconfig

- 커맨드라인에서 ipconfig 명령어를 통해 해당 기기가 접속해 있는 ip 주소를 확인할 수 있다.
- 가상머신을 돌리고 있다면, 그것의 ip 주소도 확인할 수 있다.
- 로컬 pc와 가상머신의 ip 주소를 혼동하지 말 것.

<br>
<br>
