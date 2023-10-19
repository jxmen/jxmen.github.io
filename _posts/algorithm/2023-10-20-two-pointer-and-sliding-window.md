---
title: Two Pointer와 Sliding Window
categories: [algorithm]
tags: [알고리즘]
---

Two Pointer와 Sliding Window에 대해 알아보자.

### Two pointer 알고리즘이란

![two-pointer-intro](/assets/img/posts/algorithm/two-pointer-intro.png)

투 포인터(Two Pointer)는 배열이 정렬되어 있을 때, 두 개의 포인터를 이용하여 원하는 값을 찾는 알고리즘이다.

- 보통은 lt, rt등 2개의 가리키는 포인터를 사용하며, lt는 왼쪽에서 시작하여 rt는 오른쪽에서 시작한다.
- 해당 알고리즘을 사용하기 전에는 배열이 **오름차순**으로 정렬이 되어있어야 한다.

### Two Pointer 알고리즘의 시간복잡도

배열의 크기가 N일 때, `O(N)`의 시간복잡도를 가진다.

- 단순히 배열을 합치고 오름차순으로 정렬하는 문제를 퀵 정렬을 사용한다면, `O(NlogN)`의 시간복잡도를 가질 것이다.
- 만약 이를 Two Pointer 알고리즘을 사용하면 더 적은 시간복잡도(N)로 효율적으로 문제를 해결할 수 있다.

### Sliding Window 알고리즘이란

![sliding-window-intro](/assets/img/posts/algorithm/sliding-window-intro.png)

Sliding Window 알고리즘은 배열에서 특정 영역에 해당하는 곳을 window로 지정하여 밀면서 이동하여 값을 구한다.

- Two Pointer 알고리즘과 유사하지만, 길이가 가변적인 Two Pointer와 달리 **고정적인 길이**를 가진다는 특징이 있다.

### Sliding Window 알고리즘의 시간복잡도

Two Pointer와 마찬가지로 `O(N)`의 시간복잡도를 가진다.

### Reference

- https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84
- https://stalker5217.netlify.app/algorithm/two-pointer/
