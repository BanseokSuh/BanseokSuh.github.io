---
title: Event Loop
author: Banny
date: 2023-02-18 00:06:00 +0900
categories: [Programming, Nodejs]
tags: [Event Loop]
---

## Single thread

- Node.js는 **싱글 스레드** 언어이다.
- 전체 runtime에서 스레드가 하나만 있는 것은 아니지만, 실제 사용할 수 있는 스레드는 하나이다.
- 즉, 한 번에 하나의 작업만 수행할 수 있다는 것!<br>
  \*Javascript에서 Call Stack이 하나라는 것과 Javascript는 싱글스레드 언어라는 것은 같은 맥락이다.

<br>

## Event loop란?

- 스레드가 하나인데 어떻게 여러 요청을 받아 처리할 수 있는 것인가?
- 그것을 가능하게 해주는 것이 바로 **Event loop**이다.
- Node.js는 이벤트들을 처리하면서 비동기 api를 사용한다.
  - I/O 작업들(database, http network, fs 관련 작업 등)은 worker thread에서 동작한다.
  - worker thread가 모여 있는 곳은 worker pool이라고 한다.
- Event loop에서는 worker pool에서 동작하지 않는 것들이 동작한다.
  - callback function, response serializing 등의 작업
- Event loop에서 cpu intensive한 작업을 하게 되면?
  - 싱글 스레드를 오래 점유하고 있다면? ⇒ 다른 request 처리가 전부 멈춤
  - **물리 인스턴스를 증설(scale-out)해도 효과가 크지 않음**
  - 트래픽이 많아서 발생하는 이슈가 아니기 때문이다.

<br>

## Event loop 동작 원리

- Node.js는 **v8엔진**과 **libuv 라이브러리**로 구성되어 있다.
  - libuv는 **비동기 I/O 라이브러리**이다.
  - Node에 비동기 I/O 요청(네트워크 요청, 파일 시스템, 데이터베이스 입출력 등 외부 작업들)이 들어오면 libuv 라이브러리는 해당 요청을 수행한다.
    - libuv는 커널이 어떤 비동기 api를 지원하는지 알고 있다.
    - 커널이 해당 작업을 지원한다면 커널은 작업을 처리한다.
    - 지원하지 않는다면 libuv 라이브러리는 내부적으로 스레드풀을 생성하여 여러 스레드에게 해당 요청을 맡긴다. 해당 스레드를 워커 스레드라고 부른다.
    - libuv는 기본적으로 4개 스레드를 갖는 스레드풀을 생성한다. uv_threadpool 환경 변수를 설정해 최대 128개까지 늘릴 수 있다.
- 이벤트 루프는 Node.js가 여러 비동기 작업을 처리하게 하는 구현체다.

<br>

## 요약

\_1. Node.js는 **싱글 스레드** 기반의 언어<br>
\_2. 즉, 한 번에 하나의 작업만 실행 (main 스레드에서)<br>
\_3. 어떻게 동시적인 작업을 처리하는가? **Event Loop**를 통해서!<br>
\_4. Node.js가 I/O 작업들을 요청받으면 libuv 라이브러리는 여러 해당 작업을 **worker thread**에게 **비동기적**으로 맡김<br>
\_5. 그 외에 개발자가 만들어낸 callback function 등의 로직은 **main thread**에서 동작함<br>
\_6. Event loop에서 **cpu intensive**한 작업을 하여 스레드를 오래 점유하고 있으면?<br>
\_7. Node.js에서는 더 이상 다른 요청들을 처리하지 못함 <br>
\_8. 물리 인스턴스를 증설해도 유의미한 효과를 얻지 못함 <br>
\_9. cpu intensive한 코드를 피하자!!

<br>

## 동작 visualization

- 백문이불여일견이라고, 공부하면서 보게된 좋은 gif 파일이 있어 올린다. Event Loop를 이해하는 데에 큰 도움이 되었다. 아래 gif는 브라우저 상에서의 Event loop를 이용한 비동기 처리 과정을 보여주지만, Node.js도 흐름상 이와 다를 것이 없다.

  \_1. console.log('Hi')가 stack에 쌓여 실행<br>
  \_2. setTimeout cb가 stack에 쌓여 실행<br>
  \_3. cb는 비동기적으로 5초간 webapis에서 대기 (5초는 다음 작업과 상관 없이 counting)<br>
  \_4. console.log('JSConfEU')가 stack에 쌓여 실행<br>
  \_5. 5초 후 webapis에 있던 cb은 task queue로 넘어와 Event loop에 의해 stack에 쌓여 작업 수행<br>

<br>

<center>
<img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/219848013-30735f74-47d3-451d-8e4b-827cd6f4f367.gif">
</center>

<br>

## 추가

- 아래의 그림을 이해해보도록 하자

<strong>설명</strong>
<br>
작성중

<center>
<img alt="Event Loop" src="https://user-images.githubusercontent.com/62047302/221593232-b3c7ee27-ea90-4d19-9dd4-37a6ad650c29.jpeg">
</center>

<br>
<br>
참고자료<br>
- <a href="https://www.youtube.com/watch?v=8aGhZQkoFbQ">What the heck is the event loop anyway? | Philip Roberts | JSConf EU</a>
