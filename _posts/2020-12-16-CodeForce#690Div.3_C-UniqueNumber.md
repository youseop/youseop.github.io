---
layout: post
title: CodeForce#690Div.3 C-UniqueNumber
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/codeforce.jpg
share-img: /assets/img/posting/mountain.jpg
tags: [DFS, CodeForce]
comments: true
---

## [C-UniqueNumber](https://codeforces.com/contest/1462/problem/C)

처음으로 CodeForce Contest에 참가해 보았다.

Div(1~3) 별로 난이도가 나뉘는데 이중 제일 쉬운 Div3를 시도해 봤다.

총 일곱문제가 출제되었고, 이중 3문제 밖에 풀지 못했다.

첫 시도 치고 나쁘지 않은 것 같지만, 앞으로 자극받고 더 열심히 공부하는 계기로 삼아야겠다. 문제 난이도는 그렇게 어렵다고 느껴지진 않았다. 내 실력이 부족할 뿐...

E1문제를 풀고 포스팅해보려 했지만, 계속 시간초과의 벽에 부딪히는 바람에 C-Unique Number 문제로 포스팅할 예정이다.

### 문제 해설

각각의 인풋 X가 주어졌을 때, 숫자의 각 자리수가 중복되지 않으면서, 모두 더했을 때 X가 되는 가장 작은 수를 찾는 문제이다.

먼저, 10보다 작은 인풋이 주어졌을 때는 이 숫자를 바로 출력하고, 45 = sum(range(1,10)) 보다 큰 경우에는 Unique Number를 만들 수 없음으로 -1을 출력했다.

(숫자가 중복되지 않는 수를 만들어야 하는데, 1부터 9까지의 숫자를 모두 사용해도 각 자릿수의 합이 45를 넘지 못하기 때문이다.)

dfs를 진행하며, 각각의 자릿수에 들어갈 수 있는 수들을 찾아냈다.

각 자릿수를 모두 합해서 X가 되는 수들을 배열에 저장해 놓은 후에 이들 중 최솟값을 출력했다.

```
def dfs(x,save,sum):
	#각 자릿수의 합이 n을 초과할 경우 탐색을 중지한다.
    if sum>n:
        return
    #각 자릿수의 합이 n인 경우 배열에 저장한다.
    elif sum == n:
        answer.append(int(save))
        return
	#직전에 추가된 숫자보다 큰 수들만 추가해준다.
    for i in range(x+1,10):
        if sum+i<=n:
            dfs(i,save+str(i),sum+i)
```

1) 각 자릿수의 합이 n을 초과하는 경우에는 탐색을 중지한다.

2) 각 자릿수의 합이 n인 경우 answer에 숫자를 추가해준다. (탐색이 끝난 후 answer배열 내의 최솟값 출력)

3) 직전에 추가된 수보다 큰 수들만 추가해 나간다.

-   1, 2, 3으로 구성된 자릿수의 합이 6인 수의 경우 123, 132, 213, 231, 312, 321 이렇게 6가지 조합이 가능하다.
-   최솟값은 123으로 자릿수들이 오름차순 정렬된 경우임을 알 수 있다.
-   따라서, 직전에 추가된 수보다 큰 수들만 추가해 나가는 경우가 특정 숫자 조합 내에서 최솟값을 가지는 경우이다.

---

### CODE

```
import sys, math
read=sys.stdin.readline
from collections import deque

def dfs(x,save,sum):
	#각 자릿수의 합이 n을 초과할 경우 탐색을 중지한다.
    if sum>n:
        return
    #각 자릿수의 합이 n인 경우 배열에 저장한다.
    elif sum == n:
        answer.append(int(save))
        return
	#직전에 추가된 숫자보다 큰 수들만 추가해준다.
    for i in range(x+1,10):
        if sum+i<=n:
            dfs(i,save+str(i),sum+i)

N = int(read())
for _ in range(N):
    n = int(read())
    #10 이하인 경우 해당 수를 바로 출력
    if n < 10:
        print(n)
        continue
   	#45초과인 경우 자릿수에 중복이 없는 수를 만들 수 없다.
    elif n > 45:
        print(-1)
        continue
    answer=[]
    dfs(0,"",0)
    print(min(answer))
```

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!

감사합니다.👍

\[꼭 다시 풀어보기\]