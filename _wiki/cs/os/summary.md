---
layout  : wiki
title   : 운영체제 용어 정리
summary : 
date    : 2024-02-26 02:15:02 +0900
updated : 2024-02-26 15:03:44 +0900
tag     : 
toc     : true
public  : true
parent  : [[/cs]]
latex   : false
resource: BE2D7A16-1606-4F42-9483-DCDF84E2AC77
---
* TOC
{:toc}

### 용어 정리

용어만 간단하게 정리한다. 각 용어에 대해서는 추가로 더 공부가 필요하다.

- 경쟁 상태 (race condition)
	- 여러 프로세스/스레드가 동시에 데이터 조작 시 타이밍/순서에 따라 결과가 달라질 수 있는 상태
- 동기화 (synchronization)
	- 여러 프로세스/스레드를 동시에 실행 시 **일관성**을 보장하는 과정
- 임계 영역 (critical section)
  - 하나의 스레드/프로세스만 접근 가능한 영역
- 상호 배제 (mutual exclustion)
	- 공유된 자원에 동시에 접근하지 못하도록 하는 것
	- 상호 배제 방법
		- 스핀락 (Spin Lock)
			- 락을 획득할 때 까지 계속해서 확인하는 작업
				- CPU 낭비가 심한 단점이 있음
		- 뮤텍스 (mutex)	
			- 락이 준비되었을 때 깨어나도록 대기 작업을 큐에 넣고 처리하는 방법
		- 세마포 (semaphore)
			- 뮤텍스와 유사하지만, 작업 간의 실행 순서를 기록하고 동기화하는 방법
		- 모니터 (monitor)

### 참고

- [쉬운코드 유튜브 - 스핀락, 뮤텍스, 세마포](https://www.youtube.com/watch?v=gTkvX2Awj6g)
- [운영체제 - 한빛아카데미]

