---
layout  : wiki
title   : 데이터베이스 용어 정리
summary : 
date    : 2024-02-26 22:36:23 +0900
updated : 2024-02-27 00:49:31 +0900
tag     : database 
toc     : true
public  : true
parent  : [[/database]] 
latex   : false
resource: 6C3178ED-080D-44E6-9D91-D0FB7C597D1F
---
* TOC
{:toc}

### 용어 정리

용어들만 간단하게 정리

- 낙관적 락 (Optimisstic Lock)
	- 데이터 갱신 시 경합이 발생하지 않을 것이라 보는 것
	- 버전을 명시하여, 데이터 변경 시 버전 업데이트를 통해 다른 트랜잭션이 업데이트 시도 시 예외 발생
	- 어플리케이션에서 롤백을 수동으로 해야 하는 특징이 있으나, 비관적 락에 비해 성능이 더 좋음
- 비관적 락 (Pessimistic Lock)
	- 동일한 데이터를 동시에 수정할 가능성이 높다고 보고, 무조건 락을 획득할 때 까지 대기해야 하는 잠금
	- 베타 락 (Exclusive Lock, X Lock)
		- 쓰기 잠금
	- 공유 락 (Shared Lock, S Lock)
		- 읽기 잠금
		- 읽기 잠금이 걸려 있어도 다른 읽기 잠금을 거는 것이 가능하다.

- 파티셔닝 (partitioning)
  - 데이터베이스 table을 더 작은 table로 나누는 것
- 수직 분할 (vertical partitioning)
	- column을 기준으로 table을 나누는 방식
	- 정규화도 일종의 vertial partitioning이라고 볼 수 있다.
	- select 문을 수행할 때 특정 컬럼만 지정하는 경우, 모든 열을 메모리에 불러와 잘라내는 형식으로 동작하기에 불필요한 열이 포함될 경우 성능이 저하될 가능성이 있다.
- 수평 분할 (horizontal partitioning)
	- row를 기준으로 table을 나누는 방식
	- 해시 기반 수평 분할 시, 해시 함수에 넣을 컬럼을 `파티션 키`라고 한다.
- 샤딩 (sharding)
	- horizontal partition과 유사하게 동작하나, 각 partion들을 독립적인 DB 서버로 분할하는 것.
	- (부하 분산이 목적)
	- 독립적으로 나눠진 파티션들을 (shard), 파티션의 키를 샤드 키(shard key) 라고 함
- 레플리케이션 (replication)
	- 데이터베이스 데이터를 복제하여 여러 보조 서버를 띄우는 것
	- 원본 서버는 master/primary/leader 등으로 불리고 복제된 서버는 slave/secondary/replica 등으로 불린다.
	- 원본 서버에 장애 발생 시 보조 서버가 원본 서버로 승격하여 고가용성(High availability, HA)을 보장한다.

### 참고

- [[10분 테코톡] 마루의 데이터베이스 Lock
- <https://sabarada.tistory.com/175>
- [쉬운 코드 유튜브 - 파티셔닝,샤딩,레플리케이션](https://www.youtube.com/watch?v=P7LqaEO-nGU&t=8s)
](https://www.youtube.com/watch?v=ZXV6ZqMyJLg&t=359s) 
