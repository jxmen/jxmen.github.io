---
layout  : wiki
title   : free
summary : 시스템의 메모리 정보 확인
date    : 2024-09-01 14:48:07 +0900
updated : 2024-09-01 22:15:05 +0900
tag     : commands
toc     : true
public  : true
parent  : [[/commands]]
latex   : false
resource: 47502EE6-1FFD-4C6E-8CC3-353340DD2B7B
---
* TOC
{:toc}

### 개요

현재 컴퓨터의 메모리 사용량을 확인할 수 있다. top 명령어 사용시에 상단에 나오는 메모리 정보이고, 옵션을 통해 더 다양한 형태로 볼 수 있다.

![image](https://github.com/user-attachments/assets/0168327c-dba5-4ae9-97f0-1f7032219066)

- MacOS에서는 사용할 수 없다.

### 주요 사용법 및 옵션

#### 사람이 읽기 좋은 형태로 보고 싶어요 `-h`

뒤에 인자를 지정하지 않으면 서버 용량에 따라 자동으로 읽기 좋은 형태로 지정된다.

```bash
free -h
```

#### n초마다 갱신해서 보고싶어요 `-s`

1초마다 갱신

```bash
free -s 1
```

### 내가 자주 쓰는 조합

- `free -h -s 1`: 읽기 좋은 형태로 지속적으로 보고 싶을 때 사용
- `free -s 1`: 메모리의 상세한 정보를 지속적으로 보고 싶을 때 사용

### 같이 보면 좋은 명령어

- [[/commands/top]]

