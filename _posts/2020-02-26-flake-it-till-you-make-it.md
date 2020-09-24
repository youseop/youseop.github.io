---
layout: post
title: BAEKJOON 1021
subtitle: 'deque' 회전하는 큐
cover-img: /assets/img/avatar-icon.jpg
thumbnail-img: /assets/img/posting/baekjoon.png
share-img: /assets/img/avatar-icon.jpg
tags: [que, BAEKJOON]
comments: true
---

Under what circumstances should we step off a path? When is it essential that we finish what we start? If I bought a bag of peanuts and had an allergic reaction, no one would fault me if I threw it out. If I ended a relationship with a woman who hit me, no one would say that I had a commitment problem. But if I walk away from a seemingly secure route because my soul has other ideas, I am a flake?

## [BAEKJOON #1021](https://www.acmicpc.net/problem/1021)

CASE 1 - using deque

```python
n,m= map(int,input().split())
pick=list(map(int,input().split()))
number=list(range(1,n+1))

from collections import deque
cnt=0
_pick=deque(pick)
_number=deque(number)

while _pick:
    a=_pick[0]
    while _number[0]!=a:
        idx=_number.index(a)
        if idx <= len(_number)//2:
            _number.append(_number.popleft())
        else:
            _number.appendleft(_number.pop())
        cnt+=1
    _number.popleft()
    _pick.popleft()
print(cnt)
```

CASE 2 - using list

```python
n,m= map(int,input().split())
pick=list(map(int,input().split()))
number=list(range(1,n+1))

cnt=0

while pick:
    a=pick[0]
    while number[0]!=a:
        idx=number.index(a)
        if idx <= len(number)//2:
            number.append(number.pop(0))
        else:
            number.insert(0,number.pop())
        cnt+=1
    number.pop(0)
    pick.pop(0)
print(cnt)
```
