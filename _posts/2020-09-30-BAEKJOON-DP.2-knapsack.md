---
layout: post
title: Knapsack Problem
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/baekjoonrect.png
share-img: /assets/img/posting/mountain.jpg
tags: [DP, BAEKJOON]
comments: true
---

## [평범한 배낭](https://www.acmicpc.net/problem/12865)

LIS - Longest Increasing Subsequence에 이어 계속해서 DP 대표적인 문제들에 대해 알아보자.
평범한 배낭 문제는 Knapsack Problem이라고 알려져 있는 DP를 활용한 문제다.

<br>

### Code

```python
import sys
n,k=map(int,sys.stdin.readline().split())
things=list(list(map(int,sys.stdin.readline().split())) for _ in range(n))

sack=list(0 for _ in range(k+1))
#things.sort(key=lambda x : x[0])
for i in range(n):
    for j in range(k-things[i][0],-1,-1): #j를 큰 수부터 거꾸로 보내줘야 한다.
        sack[things[i][0]+j]=max(sack[things[i][0]+j],things[i][1]+sack[j])
    #print(sack)
print(max(sack))
```

```
[0, 0, 0, 6, 6, 6, 6, 6]
[0, 0, 0, 6, 8, 8, 8, 14]
[0, 0, 0, 6, 8, 12, 12, 14]
[0, 0, 0, 6, 8, 12, 13, 14]
```

### 반례를 보여주기 위해 작성한 코드

```python
import sys
n,k=map(int,sys.stdin.readline().split())
things=list(list(map(int,sys.stdin.readline().split())) for _ in range(n))

sack=list(0 for _ in range(k+1))
things.sort(key=lambda x : x[0])
for i in range(n):
    for j in range(k-things[i][0]+1):
        sack[things[i][0]+j]=max(sack[things[i][0]+j],things[i][1]+sack[j])
    print(sack)
print(max(sack))
```

```
[0, 0, 0, 6, 6, 6, 12, 12]
[0, 0, 0, 6, 8, 8, 12, 14]
[0, 0, 0, 6, 8, 12, 12, 14]
[0, 0, 0, 6, 8, 12, 13, 14]
```

<br>

### DP 대표 유형

- Knapsack Problem (완료)
- Longest Common Sequence
- [Longest Increasing Subsequence](https://youseop.github.io/2020-09-29-BAEKJOON-DP.1-LIS/) (완료)
- Edit distance
- Matrix Chain Multiplication

[다시 풀어보기]
<br>
<br>
