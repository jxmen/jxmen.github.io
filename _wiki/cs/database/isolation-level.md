---
layout  : wiki
title   : Isolation Level 정리
summary :  
date    : 2024-02-23 02:15:21 +0900
updated : 2024-02-23 03:06:27 +0900
tag     : database transaction
toc     : true
public  : true
parent  : [[/cs/database]]
latex   : false
resource: B7FD26CE-5E2D-4415-B095-D46539ABFE8B
---
* TOC
{:toc}

### 표준 SQL 에서의 Isolation Level

표준 SQL에서는 다음과 같이 4단계의 Isloation Level을 정의한다.

- `Read uncommitted`
- `Read committed`
- `Repeatable Read`
- `Serializble`

### 트랜잭션의 이상 현상

트랜잭션 격리 레벨에 따라 이상 현상이 발생할 수 있다.

#### Dirty Read

서로 다른 두 트랜잭션에서 커밋되지 않은 내용에 대해 읽을 때 발생하는 현상이다.

#### Non-Repeatable Read 

한 트랜잭션 내에서 다른 트랜잭션이 커밋된 내용이 읽힐 때 발생하는 현상이다.

client1
```sql
START TRANSACTION;

SELECT * FROM test_user WHERE age = 10 ;

COMMIT;
```

client 2
```sql
START TRANSACTION;

UPDATE test_user SET age = 100 WHERE id = 5
;

COMMIT;
```

다음과 같은 2개의 클라이언트가 있다고 가정할 때, client1이 client2가 update 후 커밋을 후에 다시 조회하게 되면 두번의 select 했을 때의 결과가 서로 달라진다.

#### Phantom Read 

한 트랜잭션에서 조회 시 다른 트랜잭션에서 새로 추가/업데이트/삭제로 인해 기존에 없던 행이 생기는 이상 현상이다.

client1
```sql
# phantom read test
START TRANSACTION;

select * from test_user where age = 10 ;

commit;
```

client2
```sql
START TRANSACTION;

INSERT INTO test_user (age) VALUES (10);

COMMIT;
```

client1 트랜잭션 시작 -> client1 조회 -> client2 트랜잭션 시작 -> client2 insert -> client2 commit -> client1 조회 시 기존에 조회되지 않던 **유령**의 행이 추가로 발생한다.

### Isolation Level에 따른 이상현상

Isolation Level을 어떻게 설정하느냐에 따라 발생할 이상 현상을 제어할 수 있다.


| 레벨/현상           | Dirty Read | Non-Repeatable Read | Phantom Read |
|---------------------|------------|---------------------|--------------|
| Read UnCommitted    | o          | o                   | o            |
| Read Committed      | x          | o                   | o            |
| Repeatable Read     | x          | x                   | o            |
| Serializable        |     x      |          x          |       x      |

아래로 갈수록 더 많은 이상현상을 제어할 수 있으나, 그만큼 데이터베이스의 처리량(throughput)이 감소하여 성능이 떨어지게 된다.

- (추가) MySQL 8.0 버전의 경우 기본적으로 `Repeatable`레벨로 설정되지만, InnoDB 엔진은 별도의 락 메커니즘을 통해 Phantom Read 현상도 방지한다. [^1]

### 트랜잭션 레벨 보는법

#### MySQL 8.0

MySQL의 경우 다음과 같이 사용한다.

```bash
SHOW VARIABLES LIKE 'transaction_isolation';
```

```
| variable_name         | value         |
|-----------------------|---------------|
| transaction_isolation | READ-COMMITED |
```

혹은, 트랜잭션 시작 후에 사용할 Isolation Level을 설정할 수도 있다. [^2]

```bash
mysql> START TRANSACTION;
Query OK, 0 rows affected (0.02 sec)

mysql> SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
ERROR 1568 (25001): Transaction characteristics can't be changed
while a transaction is in progress
```

### 참고 사항

위의 내용은 모두 `DataGrip`, `MySQL 8.0` 기준으로 실험하였다.

### 참고 자료

- [쉬운코드 - isolation level 정리](https://youtu.be/bLLarZTrebU?si=udvNjIUCQUEs0DGt)

### TODO

- [ ] 스냅샷 레벨 추가
- [ ] 표준 SQL 비판 관련 내용 추가
 
### 각주

[^1]: <https://dev.mysql.com/doc/refman/8.0/en/innodb-next-key-locking.html>
[^2]: <https://dev.mysql.com/doc/refman/8.0/en/set-transaction.html>

