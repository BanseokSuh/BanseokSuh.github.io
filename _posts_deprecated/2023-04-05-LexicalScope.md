---
title: Lexical Scope
author: Banny
date: 2023-04-03 23:43:00 +0900
categories: [Programming, Javascript]
tags: [Lexical]
---

## 예제 코드

```js
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```

<br>

## 함수 bar의 상위 스코프는?

위 소스코드를 실행시킨다면 어떤 결과가 나올까?<br>
함수 bar의 상위 스코프가 무엇인지에 따라 결정된다.

무엇이 상위 스코프인지는 두 가지 기준으로 나뉜다.

- 함수를 어디서 **호출**하였는지에 따라 상위 스코프를 결정

  - 함수 bar의 상위 스코프는 함수 foo와 전역
  - 동적 스코프 (Dynamic scope)

- 함수를 어디서 **선언**하였는지에 따라 상위 스코프를 결정
  - 함수 bar의 상위 스코프는 전역
  - 정적 스코프 (Static scope)
  - **렉시컬 스코프 (Lexical scope)**

<br>

자바스크립트를 비롯한 대부분의 프로그래밍 언어는 **렉시컬 스코프**를 따른다.<br>렉시컬 스코프는 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정된다.<br>어디에서 호출하였는지는 중요하지 않다.

위 예제의 함수 bar는 전역에 선언되었다. 따라서 함수 bar의 상위 스코프는 전역 스코프가 되고,<br>위 예제는 전역 변수 x의 값 1을 두 번 출력한다.
