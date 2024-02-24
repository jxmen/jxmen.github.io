---
layout  : wiki
title   : 정렬
summary : 정렬의 종류, 언어에서 제공하는 정렬 등
date    : 2024-02-24 20:29:55 +0900
updated : 2024-02-25 01:54:14 +0900
tag     : algorithm sort 
toc     : true
public  : true
parent  : [[/cs/algorithm]]
latex   : false
resource: 6905F776-77FD-4E37-A183-5D8A27F858C5
---
* TOC
{:toc}

### 선택(selection) 정렬

원소가 들어갈 인덱스를 먼저 설정한 후, 어떤 원소가 들어갈 지 선택하는 정렬 방법이다.

```python
def selection_sort(arr):
    for i in range(0, len(arr)):
        min_idx = i

        for j in range(i, len(arr)):
            # 1. 주어진 배열 중에 최소값을 찾는다.
            if arr[j] < arr[min_idx]:
                min_idx = j

        # 2. 그 값을 맨 앞에 위치한 값과 교체한다.
        # 3. 맨 처음 위치를 뺀 배열을 같은 방법으로 교체
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

    return arr
```

#### 시간복잡도

| Best   | Avg    | Worst  |
|--------|--------|--------|
| O(N^2) | O(N^2) | O(N^2) |

- 모든 원소를 탐색하고 선택할지 결정해야 하므로 시간복잡도는 O(N^2)이 걸린다.
- 공간복잡도의 경우 해당 배열 내의 공간만큼만 차지하므로 O(N)을 차지한다.

### 삽입(insertion) 정렬

정렬된 원소에서 새로 추가될 원소를 특정 위치에 삽입하는 정렬 방법이다.

```python
def insertion_sort(arr):
	
	for cur_idx in range(1, len(arr)):
		cur = arr[cur_idx]
		prev_idx = cur_idx - 1
		
		while prev_idx >= 0 and arr[prev_idx] > cur:
			arr[prev_idx + 1] = arr[prev_idx]
			prev_idx -= 1
		
		arr[prev_idx + 1] = cur
		
	return arr
```

#### 시간복잡도

| Best   | Avg    | Worst  |
|--------|--------|--------|
| O(N)   | O(N^2) | O(N^2) |

- 단, 데이터가 조금 정렬되어있을 경우 효율적으로 동작한다.
- 공간복잡도는 배열의 크기만큼 차지하므로, O(N)

### 퀵(quick) 정렬

`pivot`이라는 기준 데이터를 설정하고, 그보다 작은 값과 큰 값을 각각 왼쪽/오른쪽에 분리 후 분할정복 방식으로 정렬하는 방법이다.

```python
def quick_sort(arr, start, end):
    # 원소가 1개일 경우 바로 종료
    if start >= end:
        return arr

    pivot = start
    left = start + 1
    right = end

    # 왼쪽에는 피벗보다 작은 수, 오른쪽은 피벗보다 큰 수
    while left <= right:
        while left <= end and arr[left] <= arr[pivot]:
            left += 1

        while right > start and arr[right] >= arr[pivot]:
            right -= 1

        if left > right:
            arr[pivot], arr[right] = arr[right], arr[pivot]
        else:
            arr[left], arr[right] = arr[right], arr[left]

    quick_sort(arr, left, right - 1)
    quick_sort(arr, right + 1, end)

    return arr

```

#### 시간복잡도

| Best        | Avg         | Worst       |
|-------------|-------------|-------------|
| O(N log(N)) | O(N log(N)) | O(N^2)      |

### 병합(merge) 정렬

중간 지점으로 좌/우로 나눈 후 서로 정렬한 뒤 다시 합치는 정렬 방법이다. 재귀함수로 간결하게 구현할 수 있다.

```python
def merge(left_half, right_half):
    result = []
    i = j = 0

    while i < len(left_half) and j < len(right_half):
        if left_half[i] < right_half[j]:
            result.append(left_half[i])
            i += 1
        else:
            result.append(right_half[j])
            j += 1

    result.extend(left_half[i:])
    result.extend(right_half[j:])

    return result


def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2

    # left, right 반으로 나눈다.
    left_half = arr[:mid]
    right_half = arr[mid:]

    left_half = merge_sort(left_half)
    right_half = merge_sort(right_half)

    # 왼쪽과 오른쪽을 합친다.
    return merge(left_half, right_half)
```

### 시간복잡도

| Best        | Avg         | Worst       |
|-------------|-------------|-------------|
| O(N log(N)) | O(N log(N)) | O(N log(N)) |


### 프로그래밍 언어에서 제공하는 정렬

프로그래밍 언에어 내장된 정렬들은 어떤 알고리즘을 사용할까? 궁금해서 한번 찾아보았다.

#### Java

Java의 `Arrays.sort()`메서드는 내부적으로 퀵 정렬과 삽입 정렬을 번갈아가며 사용하여, 어떤 자료형에도 관계 없이 O(n log(n)) 시간복잡도를 만족한다. 번역한 내용은 아래와 같다. [^1] 

> 이 클래스는 Vladimir Yaroslavskiy, Jon Bentley 및 Josh Bloch의 Dual-Pivot Quicksort 알고리즘의 강력하고 완전히 최적화된 순차 및 병렬 버전을 구현합니다. 

> 이 알고리즘은 모든 데이터 세트에 대해 O(n log(n)) 성능을 제공하며 일반적으로 기존(1-피벗) Quicksort 구현보다 빠릅니다. 혼합 삽입 정렬, 실행 병합 및 힙 정렬, 계산 정렬, 병렬 병합 정렬과 같이 Dual-Pivot Quicksort에서 호출되는 추가 알고리즘도 있습니다.

```java
/**
 * Sorts the specified array into ascending numerical order.
 *
 * @implNote The sorting algorithm is a Dual-Pivot Quicksort
 * by Vladimir Yaroslavskiy, Jon Bentley, and Joshua Bloch. This algorithm
 * offers O(n log(n)) performance on all data sets, and is typically
 * faster than traditional (one-pivot) Quicksort implementations.
 *
 * @param a the array to be sorted
 */
public static void sort(long[] a) {
		DualPivotQuicksort.sort(a, 0, 0, a.length);
}
```

#### Python

Python의 경우 병합 정렬과 삽입 정렬을 사용한다.


### ToDo

- [ ] 버블 정렬
- [ ] 힙 정렬
- [ ] 계수 정렬
- [ ] 기수 정렬

### 각주

[^1]: <https://github.com/openjdk/jdk/blob/d10f277bd39bb5ac9bd48939c916de607fef8ace/src/java.base/share/classes/java/util/Arrays.java#L136> 
