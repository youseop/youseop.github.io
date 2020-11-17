---
layout: post
title: BAEKJOON#1260 BFS와 DFS
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/baekjoonrect.png
share-img: /assets/img/posting/mountain.jpg
tags: [DP, BAEKJOON]
comments: true
---

## [BFS와 DFS](https://www.acmicpc.net/problem/1260)

한 달 전쯤 풀다가 실패한 후 BFS와 DFS문제를 집중적으로 연습하고 오늘 다시 도전했다.
굉장히 기본적인 문제라 생각하고 풀었지만, 반례를 생각하지 못해 런타임 에러로 고생을 했다.

- 시작 노드에서 뻗는 간선이 한 개도 없는 경우를 꼭 생각해서 코드를 짜자

먼저 간선의 정보는 시간복잡도를 줄이기 위해 dictionary자료형을 사용해 저장했다.
배열의 형태로 저장하면, 하나의 노드에서 이어지는 점을 탐색할 때 마다 최악의 경우 노드 개수만큼 탐색해야 함으로 시간복잡도가 크다.

아래에서 632ms는 배열을 사용한 경우, 112ms는 dictionary를 사용한 경우이다.

![Crepe](https://i.imgur.com/NOaBjIJ.jpg)

### DFS

- 노드를 방문했는지, 방문하지 않았는지 확인하는 check 배열을 활용한다.
- DFS를 활용해 방문하지 않은 노드들 중에 값이 가장 작은 노드를 탐색해 나간다.
- 한 점을 방문할 때 마다 해당 노드를 출력하고, DFS가 끝나면 print()를 활용해 엔터를 한 번 쳐준다.
- 도중에 방문할 수 있는 아직 방문하지 않은 노드가 없다면, DFS를 종료한다.

```python
def DFS(v):
    flag=1
    print(v,end=' ')
    if check[v]==0:
        check[v]=1
    if v in bridge:
        for i in bridge[v]:
            if check[i]==0:
                DFS(i)
                flag=2
    if flag==1:
        return

check=[0 for _ in range(n+1)]
DFS(v)
print()
```

### BFS

- 선입선출의 deque를 활용한다.
- BFS를 진행하며 주의할 점은 '현재 deque에 추가되어 있는 노드들을 모두 탐색했는데도 추가로 갈 수 있는 노드가 없을 때' BFS탐색을 멈춰야 한다는 것이다.
- 이를 위해서 while문을 돌 때, deque에 노드가 몇 개 담겨있는지 확인한 후 그 노드를 모두 탐색할 때 까지 탐색을 종료하지 못하도록 했다.

```python
from collections import deque

check=[0 for _ in range(n+1)]
check[v]=1
point=deque([v])

while point:
    flag=1
    for _ in range(len(point)):
        a=point.popleft()
        print(a,end=' ')
        if a in bridge:
            for i in bridge[a]:
                if check[i]==0:
                    point.append(i)
                    check[i]=1
                    flag=2
    if flag==1:
        break
```

### CODE

```python
import sys
read = sys.stdin.readline
#DFS에서 재귀제한을 풀어주기 위함
sys.setrecursionlimit(100000)
n,m,v=map(int,read().split())
#간선 저장
bridge=dict()
#양방향 간선임으로, a와 b 모두에 저장한다.
for i in range(m):
    a,b=map(int,read().split())
    if a in bridge:
        bridge[a].append(b)
    else:
        bridge[a]=[b]
    if b in bridge:
        bridge[b].append(a)
    else:
        bridge[b]=[a]
#방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문
for i in bridge:
    bridge[i].sort()
#DFS
def DFS(v):
    flag=1
    print(v,end=' ')
    if check[v]==0:
        check[v]=1
    if v in bridge:
        for i in bridge[v]:
            if check[i]==0:
                DFS(i)
                flag=2
    if flag==1:
        return

check=[0 for _ in range(n+1)]
DFS(v)
print()

#BFS
from collections import deque

check=[0 for _ in range(n+1)]
check[v]=1
point=deque([v])

while point:
    flag=1
    for _ in range(len(point)):
        a=point.popleft()
        print(a,end=' ')
        if a in bridge:
            for i in bridge[a]:
                if check[i]==0:
                    point.append(i)
                    check[i]=1
                    flag=2
    if flag==1:
        break
```

<br>

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!
감사합니다.👍

[꼭 다시 풀어보기]
<br>
<br>
