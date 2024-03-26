---
layout  : wiki
title   : Binary Search Tree 
summary : 
date    : 2024-03-26 16:12:07 +0900
updated : 2024-03-26 17:55:11 +0900
tag     : 
toc     : true
public  : true
parent  : [[/cs/data-structure]] 
latex   : false
resource: 2AE3383F-8DA3-4FC6-B46B-402E6521C752
---
* TOC
{:toc}

### BST(Binary Search Tree)란?

Binary Tree + 특정한 룰

- 왼쪽 SubTree는 반드시 지금 노드의 값보다 작은 값
- 오른쪽 SubTree는 반드시 지금 노드의 값보다 큰 값

예시

```
      5
     / \
    /   \
   3     7
  / \
 /   \
1     4
```

### BST의 시간복잡도

BST에서 어떤 값이 존재하는지 확인하기 위해서는, 트리의 **최대 깊이**만큼만 탐색해서 찾을 수 있다.

N개의 노드가 밸런싱되어 꽉찬 경우에도 시간복잡도는 `O(logN)`이 소요된다.

### 정렬된 배열에서도 Binary Search가 가능한데 BST를 쓰는 이유는?

이미 정렬된 배열이라도 사실 Binary Search 가 가능하다.

```
1 2 5 7 9 10 
```

하지만 배열은 삽입/삭제 등은 기존에 저장된 데이터를 밀어내는 과정이 추가로 필요하므로 `O(N)`의 시간복잡도를 가지게 된다.

BST는 삽입/삭제 역시 `O(logN)`만에 수행할 수 있기 때문에 효과적이다. (연결된 노드를 끊어버리기만 하면 된다.)

### BST의 삭제

삭제의 경우가 조금 신경써야 할 부분이 있다. 삭제가 되었을때도 BST의 형태를 유지해야 한다.

자식이 없는 경우 - 삭제할 대상을 NULL로 변경하기만 하면 된다.
```
  50         50
 /  \   ->  /  \
30  70    NULL 70
```

자식이 하나만 있는 경우 - 삭제하려는 노드의 자식들을 부모의 자식으로 연결한다.

```
    50                    50
   /  \                  /  \
  30  70 (30 제거) ->   40   70
 /
40
```

자식이 둘 다 있는 경우 - 삭제하려는 대상의 오른쪽 자식에서 가장 작은 값으로 대체한다.

```
  50                  70
 /  \                /  \
30  80  (50 제거) -> 30  80
   /  \                   \
  70  90                  90
```

### Self-Balancing

극단적으로는 다음과 같은 형태도 BST에 속한다.

```
1
 \
  2
   \ 
    3...
```

트리의 깊이가 계속해서 깊어져 검색 효율이 좋지 못할 수 있다. 이를 방지하기 위해 밸런스를 알아서 잡는 트리들이 존재한다.

- AVL Tree
- Red-Black Tree

### 참고자료

- [코맹탈출 - 자료구조 Binary Search Tree](https://www.youtube.com/watch?v=wQwB5gdnEDg)

### ToDo

- [ ] AVL Tree
- [ ] Red-Black Tree

