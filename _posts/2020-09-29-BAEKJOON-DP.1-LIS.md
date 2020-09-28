---
layout: post
title: LIS - Longest Increasing Subsequence
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/baekjoonrect.png
share-img: /assets/img/posting/mountain.jpg
tags: [DP, BAEKJOON]
comments: true
---

## [#11053 가장 긴 증가하는 부분 수열](https://www.acmicpc.net/problem/11053)

LIS - Longest Increasing Subsequence 관련 문제 #11053, #11054에 대해 다뤄보자.

수열이 주어졌을 때, 수열 내의 가장 긴 증가하는 부분 수열의 길이를 구하는 문제다. 예시에 나와 있듯이, A = {10, 20, 10, 30, 20, 50} 일 때, 가장 긴 증가하는 부분 수열은 10,20,30,50로 답은 4이다.

처음에 DP식은 해당 수열의 값 보다 작은 값들 중 가장 긴 수열의 길이를 저장하고 있는 값에 1을 더하도록 설계했다. 예제를 통해 설명하자면

A = {10, 20, 10, 30, 20, 50}

- 먼저 가장 앞에 있는 10은 10보다 작은 값들 중 수열의 길이가 저장된 항이 없음으로 0+1=1 이 입력된다. DP[10]=1
- 20은 20보다 작은 값들 중 가장 긴 수열의 길이가 DP[10]에 저장되어 있는 1 임으로 1+1=2가 입력된다. DP[20]=2
- 세번째로 입력된 10은 첫번째 10과 마찬가지로 10보다 작은 값들의 수열의 길이가 모두 0으로 저장되어 있음으로 DP[10]=1
- 30은 30보다 작은 값들 중 가장 긴 수열의 길이가 DP[20]에 저장되어 있는 2 임으로 1+2=3가 입력된다. DP[30]=3
- 이와 같은 방식으로 DP[20]=1+1=2, DP[50]=3+1=4 따라서 정답은 4가 도출된다.

코드는 다음과 같다.
<br>

### Code

```python
import sys
n=int(input())
numbers=list(map(int,sys.stdin.readline().split()))
DP=list(0 for _ in range(max(numbers)))

for num in numbers:
    DP[num]=max(DP[:num])+1 #num보다 작은 수들에 대해 최대값 탐색

print(max(DP))
```

<br>

이렇게 11053을 풀고 11054'가장 긴 바이토닉 부분 수열' 문제로 왔다.
11053에서 증가하는 수열을 찾았다면 이 문제에서는  
S1 < S2 < ... Sk-1 < Sk > Sk+1 > ... SN-1 > SN
을 만족하는 쉽게 말하면 증가하다가 감소하는 수열을 찾는 문제이다.
{50, 40, 25, 10}, {10, 20, 30, 40}와 같이 증가하거나 감소하기만 하는 수열도 포함되며, 다만 {1, 2, 3, 2, 1, 2, 3, 2, 1} 과 같은 증가하다 감소하다 다시 증가하는 이런 수열은 바이토닉 수열이 아니다.

이 문제를 풀기 위해서는 수열이 주어졌을 때 특정 값의 왼쪽에서 가장 긴 증가하는 부분 수열(11053과 같다.)을 찾고 오른쪽에서 가장 긴 감소하는 부분 수열을 구해야 했다.

여기에 11053에서 작성했던 코드를 적용시키려고 봤더니 문제가 생겼다. DP 리스트에 값을 넣을 때 주어진 순열에서 해당 숫자가 위치하는 곳을 기준으로 한 것이 아니라 그 값 자체를 위치로 사용했던 것이다.
{10, 20, 30, 40} 의 경우 '가장 긴 바이토닉 부분 수열'에 적용시키기 위해선

- DP[1]=1, DP[2]=2 , DP[3]=3 , DP[4]=4

이런식으로 값을 넣어야 하는데 위에서 내가 짠 코드는

- DP[10]=1, DP[20]=2 , DP[30]=3 , DP[40]=4

이렇게 10,20,30,40의 '값'을 기준으로 배열에 집어 넣었다.
그래서 다시 11053 가장 긴 바이토닉 부분 수열로 돌아가 코드를 다시 작성했다.

이번엔 위치를 기준으로 값을 입력했기 때문에 해당 숫자 앞에 위치하는 숫자들 중 값이 자신보다 작은 값을 탐색했다. **numbers[i] > numbers[j]** <br>
이후 자신이 저장한 값 보다 크면 그 값을 갱신시키고, **dp[i] < dp[j]**

이 과정이 모두 끝난 후 저장된 값에 1을 더해주었다. **dp[i] += 1** <br>
<br>

### code

```python
n = int(input())
numbers = list(map(int, input().split()))
dp = [0 for i in range(n)]
for i in range(n):
    for j in range(i):
        if numbers[i] > numbers[j] and dp[i] < dp[j]:
            dp[i] = dp[j]
    dp[i] += 1
print(max(dp))
```

<br>

## [#11054 가장 긴 바이토닉 부분 수열](https://www.acmicpc.net/problem/11054)

[다시 풀어보기]
<br>
<br>
