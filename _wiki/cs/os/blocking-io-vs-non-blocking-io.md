---
layout  : wiki
title   : Blocking I/O vs Non-Blocking I/O
summary : 
date    : 2024-02-26 21:24:58 +0900
updated : 2024-02-26 22:35:17 +0900
tag     : io
toc     : true
public  : true
parent  : [[/os]] 
latex   : false
resource: 8E45ABB0-BF80-436D-8C78-5A413A9D2BED
---
* TOC
{:toc}

### Blocking I/O란

Blocking I/O란 스레드가 운영체제에게 I/O 작업 수행 요청 시 해당 작업이 끝날때 까지 대기(block)해야 하는 것을 의미한다.

![blocking i/o]( /resource//dce722f0-3c36-4de6-a9e4-bd6943b34cb7.png )

### Non-Blocking I/O란

Non-Blocking I/O는 스레드가 I/O 작업 요청 시, 커널은 상태를 즉시 반환하고 I/O 작업을 수행한다. 이때 스레드는 I/O 작업 완료 유무와 관계없이 CPU 관련 작업을 할 수 있다.

![non-blocking i/o]( /resource//e9a20c5d-08ca-4ac9-b97a-b475eec0a509.png )

I/O 작업이 완료되었는지 비동기로 확인하는 요청을 보낼 수도 있고, 완료된 후 작업을 callback이나 signal을 통해 후처리할 수도 있다.

### nodejs가 싱글 스레드이지만 Non-Blocking I/O가 가능한 이유

nodejs는 이벤트 루프 기반의 자바스크립트 런타임이다.

nodejs는 I/O 작업을 내부 C++로 작성된 [libuv](https://libuv.org) 라이브러리를 통해 위임한다.

해당 라이브러가 운영체제의 커널을 통해 I/O 작업을 수행하는데, 해당 작업은 미리 만들어진 libuv 스레드풀을 통해 작업한다.

nodejs가 실행될때마다 `uv_run`이라는 메서드가 계속해서 반복하며 이벤트 루프를 처리한다. [^1]

`libuv`의 경우 I/O Multiplexing 중 `select`을 사용하여 비동기로 I/O 작업을 처리한다.

### Spring MVC vs Spring WebFlux

기존 Spring MVC는 요청이 들어올때 마다 스레드를 생성하는 `one-request-per-thread` 모델을 사용한다.

Spring 5부터 등장한 `Spring WebFlux`는 Non-Blocking I/O 기반의 웹 프레임워크를 제공한다.  [^2]

### TODO

- [ ] I/O Multiplexing 정리

### 참고

- [쉬운 코드 유튜브 - block I/O vs non-block I/O](https://www.youtube.com/watch?v=mb-QHxVfmcs&list=PLcXyemr8ZeoRRaTfapB8GMMrLMlCTN4wJ&index=4)
- [[node.js] node.js의 이벤트루프와 libuv의 이해](https://blog.naver.com/pjt3591oo/221976414901)

### 주석

[^1]: <https://github.com/nodejs/node/blob/455644582d2999728102989a93cf6b740e931418/deps/uv/src/unix/core.c#L415>
[^2]: <https://spring.io/reactive>

