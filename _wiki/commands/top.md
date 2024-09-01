---
layout  : wiki
title   : top
summary :  
date    : 2024-09-01 15:55:33 +0900
updated : 2024-09-01 15:59:11 +0900
tag     : commands 
toc     : true
public  : true
parent  : [[/commands]]
latex   : false
resource: 41CBAEFD-284A-4437-B47B-7E093EB6254D
---
* TOC
{:toc}

### 개요 

서버의 상태를 파악할 수 있는 명령어다. CPU, Memory 상태, PID 등의 다양한 정보를 제공한다.

### 옵션

#### 갱신 주기를 변경하고 싶어요 `-d`

top 명령어의 기본 d(delay-time)는 3으로 설정되어 있다. -d 옵션으로 수정 가능하다.

```bash
top -d 1
```

