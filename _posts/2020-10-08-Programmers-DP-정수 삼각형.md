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

GCD, LCM - 최대공약수와 최소공배수라는 간단해보이는 문제를 가져와 보았다. 얼핏 보면 초등학생도 풀 수 있는 간단한 문제지만 유클리드 알고리즘을 소개하기 위해 다루기로 했다.

유클리드 알고리즘을 접하기 전에는 두 수 중에서 작은 수를 찾고, 해당 수 부터 2 까지 for문을 돌리며 최대공약수를 찾았다. 이 방법도 맞지만, 주어지는 두 수가 커지면 연산량이 급격하게 증가한다는 단점이 있다. 이때, 유클리드 알고리즘(유클리드 호제법)을 활용하면 시간복잡도를 줄일 수 있다.

_유클리드 호제법_

정수 a,b에 대해서 a를 b로 나눈 나머지가 r이라고 할때, a와 b의 최대공약수를 (a,b)라고 하면

#### (a,b) = (b,r)

이 성립한다.[증명](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95)

유클리드 알고리즘(유클리드 호제법) [예시](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95)를 보며 이해해보자.
78696과 19332에 대한 최대공약수를 찾는 과정이다.

```
78696 ＝ 19332×4 ＋ 1368
19332 ＝ 1368×14 ＋ 180
1368  ＝ 180×7   ＋ 108
180   ＝ 108×1   ＋ 72
108   ＝ 72×1    ＋ 36
72    ＝ 36×2    ＋ 0
```

이를 위의 표현으로 정리하면
(78696,19332) = (19332,1368) = (1368,180) = (180,108) = (108,72) = (72,36) = (36,0)
으로 최대공약수는 36이다.

이 방식을 스왑을 이용해 코드로 구현해보자.

- x, y = y, x%y 를 통해 (a,b) = (b,r)를 구현한다.
- 위의 과정을 y가 0이 될 때 까지 반복한다.
- y가 0일 때의 x 값이 바로 최대공약수이다.

```python
def GCD(x,y):
    while(y):
        x,y=y,x%y
    return x
```

최대공약수를 구했으니 이제 최소공배수에 대해 알아보자.
C라는 최대공약수를 가지는 두 수 A,B가 있을 때, 최소공배수는 A*C*B임을 쉽게 알 수 있다. 따라서 두 수를 곱한 후 최대공약수를 나누어주면 최소공배수는 쉽게 구할 수 있다!

<br>

### CODE

```python
import sys

def GCD(x,y):
    while(y):
        x,y=y,x%y
    return x

for _ in range(int(input())):
    tmp = list(map(int,sys.stdin.readline().split()))
    tmp.sort()
    a=tmp[0]
    b=tmp[1]
    gcd=GCD(a,b)
    print(a*b//gcd)
```

<br>

[다시 풀어보기]
<br>
<br>
