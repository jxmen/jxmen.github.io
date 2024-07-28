---
layout  : wiki
title   : Java에서 우선순위 큐 사용 시 주의사항
summary : 
date    : 2024-07-28 15:25:00 +0900
updated : 2024-07-28 15:38:11 +0900
tag     : java priority-queue 
toc     : true
public  : true
parent  : [[/cs/data-structure]] 
latex   : false
resource: D36E93D3-D282-4BDF-B68C-B2B67B9BD194
---
* TOC
{:toc}

### poll 메서드와 stream

`poll()`메서드를 사용하지 않고 stream을 통해 데이터를 순회할 경우 데이터의 순서가 보장되지 않는다.

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(3);
minHeap.offer(1);
minHeap.offer(2);
minHeap.offer(4);
minHeap.offer(5);

List<Integer> minHeapStreamList = minHeap.stream()
		.mapToInt(Integer::intValue) // stream만 사용시에는 heap의 순서가 보장되지 않음
		.boxed()
		.toList();
assertThat(minHeapStreamList).containsExactly(1, 3, 2, 4, 5);
```

만약 데이터의 순서를 보장하는 식으로 구현하려면, poll 메서드를 계속해서 호출하는 식으로 코드를 작성하는 것이 좋다.

```java
// poll 사용 시 순서 보장됨과 동시에 시간 복잡도 O(1) 소요 (전체는 O(N))
List<Integer> minHeapSortedWithPoll = new ArrayList<>();
while (!minHeap.isEmpty()) {
	minHeapSortedWithPoll.add(minHeap.poll()); // O(1)
}
assertThat(minHeapSortedWithPoll).containsExactly(1, 2, 3, 4, 5);
```

전체 예제 코드는 [여기](https://github.com/jxmen/lets-coding/blob/4731e8f86cbeabe090a8c2d6e93d2aeb17686d4d/algorithm/java/src/test/java/HeapTest.java#L78)에 있다.

