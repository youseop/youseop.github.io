---
layout: post
title: BAEKJOON#9663 N-Queen
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/baekjoonrect.png
share-img: /assets/img/posting/mountain.jpg
tags: [비트연산자, BAEKJOON]
comments: true
---

## [N-Queen](https://www.acmicpc.net/problem/9663)

![Crepe](https://i.imgur.com/pTvd40m.jpg)
- 비트 연산자를 사용하고, 대칭성을 이용해 첫째 줄에서는 절반까지 탐색하도록 해서 결국 풀 수 있었다. 유독 시간제한이 심하게 빡빡한 것 같다.

<br>

크기가 N X N인 체스판 위에 퀸 N 개를 공격할 수 없도록 배치하는 문제이다.

처음 문제를 풀 때, 배열로 체스판을 실제로 만든 다음에 퀸을 가능한 자리에 배치하고 그 자리에서 공격할 수 있는 구역들을 모두 표시해 주는 식으로 풀었다.
하지만, 당연하게도 시간 초과가 발생해서 다른 방법을 찾아 풀어야 했다.

![Crepe](https://i.imgur.com/XzUWe22.jpg)

위의 그림과 같이 퀸을 특정 칸에 배치하면, 해당 칸으로부터 가로, 세로 그리고 대각선에 퀸을 놓을 수 없게 된다.

이때, 퀸을 배치한 자리로부터 한 칸씩 아래 칸으로 내려갈 때마다 대각선 성분들이 한 칸씩 좌우로 멀어져 가는 것을 이용했다.

두 개의 퀸을 한 줄에 배치할 수 없음으로, 한 줄 한 줄 내려가며 퀸을 배치할 수 있는 공간을 탐색해보자.

![Crepe](https://i.imgur.com/sAgy4qm.jpg)

- 퀸을 특정 칸예 배치하면 그 칸으로부터 수직인 칸들, 왼쪽 아래 대각선 그리고 오른쪽 아래 대각선에 다른 퀸을 배치할 수 없게 된다.
- 배치한 칸으로부터 한 칸씩 아래로 내려오게 되면, 왼쪽 대각선 성분과 오른쪽 대각선 성분이 양옆으로 한 칸씩 멀어진다.
- 이를 이용해 한 줄씩 따로 떼어서 퀸을 배치할 수 있는 공간을 찾는다.
- 퀸을 배치한 곳으로부터 왼쪽, 오른쪽 대각선 그리고 중앙의 수직인 부분들을 각각 left, right, center 배열에 저장했다.
- center 배열의 성분들은 변하지 않고, lef와 right 배열의 성분들은 한 칸씩 내려감에 따라 좌우로 한 칸씩 이동하게 된다.

---

```python
def bfs(left, center, right,cnt):
    global answer
    #n개의 queen을 모두 배치했으면, 재귀 탈출
    if cnt== n:
        answer += 1
        return
    #left성분들은 한 칸 왼쪽으로, 
    #right성분들은 한 칸 오른쪽으로 이동시켜 준다.
    for i in range(cnt):
        left[i] -= 1
        right[i] += 1

    for i in range(n):
        if i not in left and check[i] and i not in right:
            check[i]=False
            #다음칸으로 이동
            bfs(left+[i], center+[i], right+[i], cnt+1)
            check[i]=True
    return
```

- 다음과 같이 left, center 그리고 right에 퀸을 배치할 수 없는 칸들의 정보를 담았다.
- left와 right는 한 칸씩 아래로 이동함에 따라 좌우로 한 칸씩 이동시켜 주었다.

<br>

### CODE

```python
import sys
read=sys.stdin.readline

def bfs(left, center, right,cnt):
    global answer
    #n개의 queen을 모두 배치했으면, 재귀 탈출
    if cnt== n:
        answer += 1
        return
    #left성분들은 한 칸 왼쪽으로, 
    #right성분들은 한 칸 오른쪽으로 이동시켜 준다.
    for i in range(cnt):
        left[i] -= 1
        right[i] += 1

    for i in range(n):
        if i not in left and check[i] and i not in right:
            check[i]=False
            #다음칸으로 이동
            bfs(left+[i], center+[i], right+[i], cnt+1)
            check[i]=True
    return

n = int(input())

answer = 0
check=list(True for _ in range(n))

bfs([], [], [],0)
print(answer)
```
<br>

---

## 개선 1 - 비트 연산자 사용

- 비트연산자가 아직 생소하다면 [11723-집합](https://www.acmicpc.net/problem/11723)문제를 먼저 풀어보자.

비트 연산자를 사용해 시간 복잡도를 줄일 수 있다.

아래와 같이 이진수에 해당 칸에 퀸을 배치할 수 있는지 없는지 저장하자.
예를 들어서 (0,2)에 Queen이 배치된다면, left, center, right에 (1 << 2)가 더해진다. 
한 칸씩 아래로 이동함에 따라 'left = left << 1', 'right = right >> 1'이 되어 양옆으로 한 칸씩 1이 밀려나게 된다.(1이 위치하는 자리가 퀸을 배치할 수 없는 곳이다.)

![Crepe](https://i.imgur.com/itQlpue.jpg)

- 설명을 위해 2차원으로 그려놨지만, 한 줄씩 탐색해 나간다는 것을 잊지 말자.

<br>

### CODE - 비트연산자 사용

```python
import sys
read=sys.stdin.readline

def dfs(left, center, right,cnt):
    global answer
    if cnt== n:
        answer += 1
        return
    #한 칸 내려왔음으로
    #왼쪽 대각선, 오른쪽 대각선 성분을 양옆으로 한 칸씩 이동시킨다.
    right  >>= 1
    left <<= 1
    #center,right,left를 합쳐,
    #현재 줄에서 퀸을 배치할 수 없는 칸을 구한다.
    board = center | right | left
    #배치할 수 있는 곳이 없으면 탐색을 종료한다.
    if board&full==full:
        return

    for i in range(n):
        #i 번째 칸에 퀸을 놓을 예정이다.
        bit = 1 << i
        
        if not(board & bit):
            #퀸을 배치한 자리에 1을 추가하고 다음 칸으로 넘어간다.
            dfs(left | bit , center | bit , right | bit , cnt+1)
    return

answer = 0
n = int(input())
full = (1<<n)-1 #칸이 가득 차있는 경우를 확인하기 위한 변수
dfs(0,0,0,0)

print(answer)
```

<br>

---

## 개선 2 - 대칭성 이용

비트 연산자를 써도 시간 초과가 발생했다.
마지막 방법으로 대칭성을 이용해 첫 줄에서 퀸의 위치를 절반만 탐색하도록 했다.

![Crepe](https://i.imgur.com/L3sRspm.jpg)

위의 그림에서 1번 칸에 퀸이 놓였을 때 체스판에 퀸을 배치할 수 있는 경우의 수와 2번 칸에 퀸이 놓였을 때의 경우의 수는 동일하다. 좌우 대칭임으로 항상 동일하다고 할 수 있다. 같은 이유로 2와 5, 3과 4에 퀸이 놓였을 때도 경우의 수가 동일하다.

따라서, 첫 줄에 한해서 퀸의 위치를 절반만 탐색하고 이를 통해 얻은 경우의 수에 두 배를 해주면 퀸을 배치할 수 있는 경우의 수를 얻을 수 있다.

![Crepe](https://i.imgur.com/YBDY6aG.jpg)

n이 홀수일 때는 계산을 한 번 더 해주어야 한다. 
n이 짝수일 때와 마찬가지로 1과 5, 2와 4는 경우의 수가 동일하지만 3이 남는다.
따라서 홀수일 때는 첫 줄 중앙에 위치한 칸에 퀸이 위치할 때의 경우의 수를 추가로 더해주어야 한다.

<br>

### CODE - 대칭성 이용

```python
import sys
read=sys.stdin.readline

def dfs(left, center, right,cnt):
    global answer
    if cnt== n:
        answer += 2
        return
    left  >>= 1
    right <<= 1
    board = center | right | left
    if board&full==full:
        return
    if cnt ==0:
        for i in range(n//2):
            bit = 1 << i
            if not(board & bit):
                dfs(left | bit , center | bit , right | bit , cnt+1)
        return
    for i in range(n):
        bit = 1 << i
        
        if not(board & bit):
            dfs(left | bit , center | bit , right | bit , cnt+1)
    return

def dfs_odd(left, center, right,cnt):
    global answer
    if cnt== n:
        answer += 1
        return
    left  >>= 1
    right <<= 1
    board = center | right | left
    if board&full==full:
        return
    if cnt ==0:
        bit = 1 << (n//2)
        if not(board & bit):
            dfs_odd(left | bit , center | bit , right | bit , cnt+1)
        return
    for i in range(n):
        bit = 1 << i
        
        if not(board & bit):
            dfs_odd(left | bit , center | bit , right | bit , cnt+1)
    return

answer = 0
n = int(input())
full = (1<<n)-1
dfs(0,0,0,0)

if n%2 == 0:
    print(answer)
else:
    dfs_odd(0,0,0,0)
    print(answer)
```

<br>
틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!
감사합니다.👍

[꼭 다시 풀어보기]
<br>
<br>
