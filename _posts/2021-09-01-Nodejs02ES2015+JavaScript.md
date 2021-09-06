---
title: 『Node.js 교과서』 - 알아두어야 할 자바스크립트
author: Banny
date: 2021-09-01 00:00:05 +0900
categories: [NodeJS, 『Node.js 교과서』]
tags: [NodeJS, 『Node.js 교과서』]
---

## const, let - var와의 차이

var는 함수 스코프를 가집니다. 그래서 블록('{'와 '}')과 관계없이 어디서든 접근할 수 있습니다.
하지만 const와 let은 블록 스코프를 가집니다. 블록 밖에서는 접근할 수 없죠.
이처럼 const, let과 var는 스코프의 종류가 다릅니다.

const와 let은 어떨까요.
const는 한 번 값을 할당하면 다른 값을 할당할 수 없습니다. 또한 초기화할 때 값을 할당하지 않으면 에러가 발생합니다. 상수라고 불리죠.

<br>

## 템플릿 문자열

백틱(`)을 이용하여 문자열 안에 변수를 넣을 수 있습니다. 가독성에 있어서 훨씬 좋죠.

<br>

## 객체 리터럴

```js
const sayNode = function () {
  console.log("Node");
};

const es = "ES";

const newObject = {
  sayJS() {
    console.log("JS");
  },
  sayNode,
  [es + 6]: "Fantastic",
};

newObject.sayNode(); // Node
newObject.sayJS(); // JS
console.log(newObject.ES6); // Fantastic
```

먼저 객체 안의 메서드에 함수를 연결할 때 콜론과 function을 쓰지 않아도 됩니다.
또한 객체의 속성명과 변수명이 동일한 경우에는 한 번만 써도 되게 바뀌었습니다. 코드의 중복을 피할 수 있죠.

<br>

## 화살표 함수

화살표 함수가 기존 function과 다른 점은 this 바인드 방식입니다.

화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정됩니다. 동적으로 결정되는 일반 함수와는 달리 화살표 함수의 this 언제나 상위 스코프의 this를 가리키게 되는데요, 이를 Lexical this라고 합니다.

<br>

## 구조분해 할당

객체나 배열로부터 속성이나 요소를 쉽게 꺼낼 수 있습니다. 다음은 mdn에서의 예시입니다.

```js
let a, b, rest;
[a, b] = [10, 20];

console.log(a); // expected output: 10

console.log(b); // expected output: 20

[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(rest); // expected output: Array [30,40,50]
```

<br>

## 클래스

클래스 문법이 도입이 되었긴 했지만, 여전히 프로토타입 기반으로 동작합니다. 프로토타입 기반 문법을 보기 좋게 클래스로 바꾼 것이라고 이해하면 되겠습니다.

이 부분은 따로 블로깅하도록 하겠습니다.

<br>

## 프로미스

자바스크립트와 노드에서는 주로 비동기를 접합니다. 많은 API들이 콜백 대신 프로미스(Promise) 기반으로 재구성되며, 악명 높은 콜백 지옥(callback hell) 현상을 극복했다는 평가를 받고 있습니다.

프로미스를 쉽게 설명하면, 실행은 하되 결괏값은 나중에 받는 객체입니다. 결과값은 실행이 완료된 후 then이나 catch 메서드를 통해 받습니다.

```js
const condition = true;
const promise = new Promise((resolve, reject) => {
  if (condition) {
    resolve("성공");
  } else {
    reject("실패");
  }
});
// 다른 코드 들어갈 수 있음
promise
  .then((message) => {
    console.log(message); // 성공(resolve)한 경우 실행
  })
  .catch((error) => {
    console.error(error); // 실패(reject)한 경우 실행
  })
  .finally(() => {
    console.log("무조건"); // 끝나고 무조건 실행
  });
```

<br>

## async/await

프로미스가 콜백 지옥을 해결했다지만, 여전히 코드가 장황합니다. then과 catch가 계속 반복되기 때문인데요, async/await은 promise 문법을 더 깔끔하게 줄여줍니다.

방법은 간단합니다. 함수 선언부를 일반 함수 대신 async function으로 교체한 후, 프로미스 앞에 await을 붙입니다.
그렇다면 함수는 해당 프로미스가 resolve될 때까지 기다린 뒤 다음 로직으로 넘어갑니다.

async/await 구문은 에러를 처리하는 부분이 없으므로 try/catch문으로 감싸줍니다. catch가 에러를 처리하게 됩니다.

이 부분은 callback, promise와 함께 따로 블로깅하도록 하겠습니다.

<br>
<br>
참고자료<br>
- <a href="http://www.yes24.com/Product/Goods/62597864">『Node.js 교과서』 - 조현영</a>
