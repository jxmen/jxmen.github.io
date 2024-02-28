---
layout  : wiki
title   : (작성중) CPU Bound vs I/O Bound 
summary : 
date    : 2024-02-28 02:03:56 +0900
updated : 2024-02-28 02:21:07 +0900
tag     : cpu io
toc     : true
public  : true
parent  : [[/cs/os]]
latex   : false
resource: 0743A533-4803-41AA-A277-2657D91E1F67
---
* TOC
{:toc}

### Brust

어떠한 **현상이 집중적**으로 발생하는 것

- CPU Burst란?
	- '프로세스'가 CPU 작업이 계속해서 일어나는 시간
- I/O Burst란?
	- '프로세스'가 I/O 작업이 끝날 때 까지 대기하는 시간

일반적으로는 CPU는 짧게 쓰고 끝나는 편. (표 참조)

### CPU Bound란

CPU Burst가 많은 것

동영상 편집 프로그램, 머신러닝 등이 해당

### I/O Bound란

I/O Burst가 많은 것?

(일반적인) 백엔드 API 서버

### CPU Bound 프로그램에서 적절한 스레드 양

CPU와 스레드 개수가 동일하거나, 조금 더 많은 양을 권장

### I/O Bound 프로그램에서 적절한 스레드 양

thread per request 모델의 경우 상황에 따라 적절하게 튜닝하는 것이 중요

- 현재 사용중인 하드웨어 스펙은 어떻게 되는가?
- I/O Burst 비율은 어떻게 되는가?
- 사용자의 트래픽 패턴은 어떻게 되는가?

개인적 견해: 성능 테스트를 해서 적정한 사이즈를 찾자.

### 참고

- <https://www.youtube.com/watch?v=qnVKEwjG_gM&t=50s>

