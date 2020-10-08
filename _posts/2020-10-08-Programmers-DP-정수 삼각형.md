---
layout: post
title: Programmers#DP 정수 삼각형
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

- 먼저 꼭대기에서 두번째 칸 까지 거쳐가는 숫자의 최대값은 경우가 한가지 뿐이다. 두번째 칸에 10, 15를 넣어주자.(왼쪽 삼각형)
- 두번째 칸에서 세번째 칸으로 이동할 때에는 양 끝 숫자들은 그대로 더해져서 들어간다. 가운데(1)칸 까지 거쳐갈 수 있는 경로의 경우의 수는 10으로 부터 오는경우, 15로 부터 오는경우 이렇게 2가지이다.
- 따라서, 10+1 과 15+1 중 최대값을 입력해준다.
- 세번째 칸과 네번째 칸에서도 동일한 방식으로 제일 마지막 줄에 20, 25, 20, 19가 입력된다.

코드를 작성할 때는 먼저 해당 칸 까지 거쳐간 숫자의 최대값을 저장할 리스트를 생성한다. (0,0)에는 꼭지점의 값을 입력해준다. 두번째 칸 부터 양 끝 점은 이전 칸의 양 끝 점에 간단히 더해주고, 중간에 있는 값들은 이전 단계의 인접한 두 숫자 중, 더 큰 값을 더해 입력해준다.
최대값들이 저장된 리스트의 최대값을 출력한다.

<br>

### CODE

```python
def solution(triangle):
    answer = 0
    n=len(triangle)

    # 해당 칸 까지 거쳐간 숫자의 최대값을 저장할 리스트 생성
    save=list(list(0 for _ in range(n)) for _ in range(n))
    save[0][0]=triangle[0][0]

    for i in range(1,n):
        for j in range(i+1):
            #양 끝 점에 대해서는 비교할 필요 없이 바로 값을 더해 입력해준다.
            if j==0:
                save[i][j]=save[i-1][j]+triangle[i][j]
            elif j==i:
                save[i][j]=save[i-1][i-1]+triangle[i][j]
            #두 값을 비교해 더 큰 숫자를 더해 입력한다.
            else:
                save[i][j]=max(triangle[i][j]+save[i-1][j],triangle[i][j]+save[i-1][j-1])

    #print(save[-1])
    answer=max(save[-1])

    return answer
```

<br>

### 다른 풀이

내 풀이는 save라는 값을 저장할 리스트를 따로 만들어서 저장했다.
하지만, 이 풀이는 triangle 리스트에 그대로 값을 더해주며 진행해 나갔다. 비슷한 방식이지만, 이 풀이가 훨씬 간단하고 한눈에 뭘 하고있는지 눈에 들어와서 가져와 보았다.
<br>

### CODE

```python
def solution(triangle):
    for t in range(1, len(triangle)):
        for i in range(t+1):
            if i == 0:
                triangle[t][0] += triangle[t-1][0]
            elif i == t:
                triangle[t][-1] += triangle[t-1][-1]
            else:
                # 이전 칸의 인접한 값들 중 더 큰 값을 해당 값에 더해서 갱신해준다.
                triangle[t][i] += max(triangle[t-1][i-1], triangle[t-1][i])
    return max(triangle[-1])

```

<br>

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
