---
layout: post
title: Programmers Dp 정수 삼각형
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/Programmers.png
share-img: /assets/img/posting/mountain.jpg
tags: [Dp, Programmers]
comments: true
---

## [정수 삼각형](https://programmers.co.kr/learn/courses/30/lessons/43105?language=python3)

백준 16236번 아기상어를 포스팅 할 예정이었지만, 거의 다 온 것 같으면서도 이상하게 풀리지가 않아 내일로 미뤄졌다.

프로그래머스 사이트에서 푼 DP 문제 정수 삼각형에 대해 알아보자.

```
[[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]]
```

다음과 같이 위에서 부터 차례대로 1개, 2개, 3개 ... 로 주어지는 삼각형 모양의 수들에 대해서 꼭대기부터 바닥까지 거쳐간 숫자의 최대값을 구하는 문제이다.

그림을 통해 알아보자.

![Crepe](https://i.imgur.com/VNI6h9r.jpg)

<br>

### CODE

```python
def solution(triangle):
    answer = 0
    n=len(triangle)
    save=list(list(0 for _ in range(n)) for _ in range(n))
    save[0][0]=triangle[0][0]

    for i in range(1,n):
        for j in range(i+1):
            if j==0:
                save[i][j]=save[i-1][j]+triangle[i][j]
            elif j==i:
                save[i][j]=save[i-1][i-1]+triangle[i][j]
            else:
                save[i][j]=max(triangle[i][j]+save[i-1][j],triangle[i][j]+save[i-1][j-1])

    print(save[-1])
    answer=max(save[-1])

    return answer
```

DP 대표유형 5가지를 다뤄보기로 했었는데 아직 Edit distance 유형을 끝내지 못했다. 이번 주 내로 다뤄봐야겠다.

### DP 대표 유형

- [Knapsack Problem](https://youseop.github.io/2020-09-30-BAEKJOON-DP.2-knapsack/) (완료)
- [Longest Common Sequence](https://youseop.github.io/2020-10-01-BAEKJOON-9251-LCS/) (완료)
- [Longest Increasing Subsequence](https://youseop.github.io/2020-09-29-BAEKJOON-DP.1-LIS/) (완료)
- Edit distance
- [Matrix Chain Multiplication](https://youseop.github.io/2020-10-02-BAEKJOON-11049-%ED%96%89%EB%A0%AC%EA%B3%B1%EC%85%88%EC%88%9C%EC%84%9C/) (완료)

<br>

[다시 풀어보기]
<br>
<br>
