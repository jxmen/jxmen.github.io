---
layout  : wiki
title   : 코딩 테스트를 위한 테크닉 및 문법 정리
summary : 
date : 2024-10-06 23:00:54 +0900
updated : 2024-10-06 23:00:59 +0900
tag     : dsa
toc     : true
public  : true
parent  : [[/dsa]] 
latex   : false
resource: EAAE0CDB-FCB2-48D8-870E-4B0FEFF04AE3
---
* TOC
{:toc}

코딩 테스트에서 자주 쓰거나 유용한 문법들을 정리합니다.

### Python

#### Swap

tmp같은 변수를 쓰지 않고 간단하게 한줄로 가능합니다.

```python
a, b = b, a
```

#### Array 마지막 인덱스 접근
Array의 마지막 인덱스 접근은 `[-1]`로 간단하게 가능

```python
stack = [1,2,3,4,5]

peek = stack[-1] # stack[len(stack) - 1]과 동일
```

#### Array Index 회전

`%` 연산을 통해, Array의 총 길이 n을 넘어가면 다시 1을 할당하도록 할 수 있습니다.

#### Heap

```
from heapq import heappush

heap = []
heapq.heappush(heap, 50) # 최소 힙에 값 넣기
```

최대 힙은 부호를 변경하여 구현한다.

```
from heapq import heappush, heappop

heap = []
heapq.heappush(heap, -value) # 최대 힙

print(-heapq.heappop(heap))
```

