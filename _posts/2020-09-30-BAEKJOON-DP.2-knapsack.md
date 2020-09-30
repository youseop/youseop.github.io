---
layout: post
title: BAEKJOON#12865 평범한 배낭
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/baekjoonrect.png
share-img: /assets/img/posting/mountain.jpg
tags: [DP, BAEKJOON]
comments: true
---

## [평범한 배낭](https://www.acmicpc.net/problem/12865)

LIS - Longest Increasing Subsequence에 이어 계속해서 DP 대표적인 문제들에 대해 알아보자.
Knapsack Problem은 크게 두가지 유형으로 나뉜다. 배낭에 들어가는 물건을 잘게 쪼갤 수 있는경우와 그렇지 않은 경우다. 전자의 경우 Greedy 알고리즘을 적용하면 쉽게 풀리며, 후자의 경우는 DP를 사용해야한다. 먼저 후자의 경우인 백준 12865번 문제를 풀어보자.

DP쪽 문제들이 코드를 완성해 놓으면 짧고 간단해 보여도 코드로 구현하기 전 생각하는 과정이 어려운 것 같다. 이 문제도 계속 틀렸다고 나오는데 반례를 찾지 못해 몇시간을 헤맷던 문제다.

먼저 가방에 들어갈 수 있는 최대 무게 만큼의 크기를 가진 배열을 만들어 준다.
_sack=list(0 for \_ in range(k+1))_
이 배열에 해당 무게에서 얻을 수 있는 최대 가치를 입력해줄 것이다.
물건별로 무게를 체크한 후 배열에 대입해준다.

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

코드의 출력을 보며 생각해보자.

- 설명을 위해 추가로 출력하는 문장을 입력했다. _print(sack)_
- 다른 코드와의 비교 설명을 위해 추가적인 정렬을 했다. _things.sort(key=lambda x : x[0])_

sack의 0-7까지 항은 모두 0으로 초기화되어있는 상태다.
첫번째로 들어온 짐의 무게는 3, 가치는 6이다. 이때, 짐의 무게 3과 더해도 최대 무게인 7을 넘지 않는 선에서 기존 sack의 값들과 더해준다. 0으로 초기화되어있는 상태라 sack[3]~sack[7]의 값은 모두 6이 되었다.

_sack에 저장되는 값_

```
[0, 0, 0, 6, 6, 6, 6, 6]
```

두번째로 들어오느 짐의 무게는 4, 가치는 8이다.
아래와 같은 연산을 거치게 된다.

- (무게 4의 가치인 )8를 (sack[3]에 들어있는 가치 6)과 더해 sack[7]의 값과 비교 >> sack[7]=max(6,6+8)=14
- (무게 4의 가치인 )8를 (sack[2]에 들어있는 가치 0)과 더해 sack[6]의 값과 비교 >> sack[6]=max(6,8)=8
- (무게 4의 가치인 )8를 (sack[1]에 들어있는 가치 0)과 더해 sack[5]의 값과 비교 >> sack[5]=max(6,8)=8
- (무게 4의 가치인 )8를 (sack[0]에 들어있는 가치 0)과 더해 sack[4]의 값과 비교 >> sack[4]=6

_sack에 저장되는 값_

```
[0, 0, 0, 6, 8, 8, 8, 14]
```

무게 5 가치 12를 가지는 세번째 짐에 대해서는 다음과 같다.

- (무게 5의 가치인 )12를 (sack[2]에 들어있는 가치 0)과 더해 sack[7]의 값과 비교 >> sack[7]=max(14,12)=14
- (무게 5의 가치인 )12를 (sack[1]에 들어있는 가치 0)과 더해 sack[6]의 값과 비교 >> sack[6]=max(8,12)=12
- (무게 5의 가치인 )12를 (sack[0]에 들어있는 가치 0)과 더해 sack[5]의 값과 비교 >> sack[5]=max(8,12)=12

_sack에 저장되는 값_

```
[0, 0, 0, 6, 8, 12, 12, 14]
```

무게 6 가치 13을 가지는 네번째 짐에 대해서는 다음과 같다.

- (무게 6의 가치인 )13을 (sack[1]에 들어있는 가치 0)과 더해 sack[7]의 값과 비교 >> sack[7]=max(14,13)=14
- (무게 6의 가치인 )13을 (sack[0]에 들어있는 가치 0)과 더해 sack[6]의 값과 비교 >> sack[6]=max(12,13)=12

_sack에 저장되는 값_

```
[0, 0, 0, 6, 8, 12, 13, 14]
```

이런 과정을 거쳐 최종적으로 최대 가치인 14를 얻을 수 있다.
위의 설명에서 j를 가능한 큰 값에서 부터 1까지 작은 값으로 줄여가며 비교를 했다.
이에 대한 이유를 아래의 코드와 출력을 보며 이해해보자.
<br>

### 반례를 보여주기 위해 작성한 코드

```python
import sys
n,k=map(int,sys.stdin.readline().split())
things=list(list(map(int,sys.stdin.readline().split())) for _ in range(n))

sack=list(0 for _ in range(k+1))
things.sort(key=lambda x : x[0])
for i in range(n):
    for j in range(k-things[i][0]+1): #위의 코드와 달리 j를 오름차순으로 대입해주었다.
        sack[things[i][0]+j]=max(sack[things[i][0]+j],things[i][1]+sack[j])
    print(sack)
print(max(sack))
```

_sack에 저장되는 값_

```
[0, 0, 0, 6, 6, 6, 12, 12]
[0, 0, 0, 6, 8, 8, 12, 14]
[0, 0, 0, 6, 8, 12, 12, 14]
[0, 0, 0, 6, 8, 12, 13, 14]
```

첫번째로 들어오는 짐의 무게는 3, 가치는 6이다. 코드에서 발생하는 일은 다음과 같다.

- (무게 3의 가치인 )6를 (sack[0]에 들어있는 가치 0)과 더해 sack[3]의 값과 비교 >> sack[3]=6
- (무게 3의 가치인 )6를 (sack[1]에 들어있는 가치 0)과 더해 sack[4]의 값과 비교 >> sack[4]=6
- (무게 3의 가치인 )6를 (sack[2]에 들어있는 가치 0)과 더해 sack[5]의 값과 비교 >> sack[5]=6
- _(무게 3의 가치인 )6를 (sack[3]에 들어있는 가치 6)과 더해 sack[6]의 값과 비교 >> sack[6]=6+6=12_
- _(무게 3의 가치인 )6를 (sack[4]에 들어있는 가치 6)과 더해 sack[7]의 값과 비교 >> sack[7]=6+6=12_

이렇게 sack[6]과 sack[7]에는 이전 과정에서 저장된 sack[3]과 sack[4]의 값이 더해져 6의 두배에 해당하는 값이 저장되어 버렸다.
위와 같은 테스트 케이스에서는 결과 값은 동일했지만 다른 테스트 케이스 에서는 다음과 같은 이유로 출력이 정답과 다를것이다.
따라서 위 코드에서 j를 내림차순으로 대입시켜 주어야 중복이 발생되지 않는다!

<br>

## [Knapsack Problem-Greedy](https://en.wikipedia.org/wiki/Knapsack_problem)

Knapsack Problem의 또다른 유형으로 전자였던 물건을 쪼갤 수 있는 경우가 있다.
이 문제같은 경우에는 입력받은 짐의 무게와 가치 값들을 무게 대비 가치가 높은 순으로 정렬시켜 준 후 차례대로 배낭에 넣으면 된다.

_things.sort(key=lambda x : -x[1]/x[0])_ <br>
무게대비 가치를 오름차순으로 정렬하기 위해 '-'를 붙여주었다.

배낭에 물건을 넣을 때 제한 무게를 초과하게 되는 경우 추가로 넣을 수 있는 무게만큼만 더 쪼개서 넣은 후 이후의 값들은 버리면 된다.

```python
import sys
n,k=map(int,sys.stdin.readline().split())
things=list(list(map(int,sys.stdin.readline().split())) for _ in range(n))

things.sort(key=lambda x : -x[1]/x[0]) # 무게 대비 가치가 높은 순으로 정렬

val=0
weight=0
for w,v in things:
    if weight+w<=k:
        val+=v
        weight+=w
    else:
        val+=(k-weight)*v/w #남은 무게만큼의 가치를 비례식 이용해 계산
        break #배낭이 꽉 찼다는 뜻임으로 for문 탈출

print(val)
```

## [Knapsack Problem 응용 - #7579 앱](https://www.acmicpc.net/problem/7579)

knapsack Problem 응용문제도 추가로 풀어봤다. 이 문제는 휴대폰의 저장공간을 확보하기 위한 최소 비용을 구하는 문제다. 가방에서 저장공간으로 단어만 바뀌었을 뿐 근본적으로는 처음에 다루었던 knapsack문제와 거의 비슷한 문제다.

#12865 평범한 배낭 문제와 같은 방법으로 처음에 접근을 했지만, 시간초과가 발생했다.
이 문제에서 저장공간으로 주어지는 M의 범위가 1 <= M <= 10,000,000이었고, 최대 10,000,001 길이를 가진 배열을 만들어 값을 집어넣는 과정을 거쳐야 했기에 시간초과가 발생했던것 같다.

### Failed code

```python
import sys
n,m=map(int,sys.stdin.readline().split())
apps=list(map(int,sys.stdin.readline().split()))
memorys=list(map(int,sys.stdin.readline().split()))

k=sum(apps)-m

kill=list(0 for _ in range(k+1))
for app,memory in zip(apps,memorys):
    for j in range(k-app,-1,-1):
        kill[j+app]=max(kill[j+app],memory+kill[j])
    print(kill) #배열 출력하기 위해 추가로 입력
print(sum(memorys)-max(kill))
```

위의 코드를 실행시켜 보면 입력 제한 대비 굉장히 작은 두 자리수의 입력임에도 불구하고 비교적 긴 리스트들이 출력된다.

![Crepe](https://i.imgur.com/sAcrHXI.jpg)

그래서 범위가 비교적 작은 N - 앱을 비활성화 했을 경우의 비용을 기준으로 배열을 만들었다.

```python
import sys
n,m=map(int,sys.stdin.readline().split())
apps=list(map(int,sys.stdin.readline().split()))
memorys=list(map(int,sys.stdin.readline().split()))

#k=m/(sum(apps)/sum(memorys))
#k=int(k)+1     #시간 효율을 개선하기위해 노력한 흔적 (성공하진 못햇다.)
k=sum(memorys)+1

kill=list(0 for _ in range(k+1))
for app,memory in zip(apps,memorys):
    for j in range(k-memory,-1,-1):
        kill[j+memory]=max(app+kill[j],kill[j+memory])
    print(kill) #배열 출력하기 위해 추가로 입력

for index,val in enumerate(kill):
    if val>=m:
        print(index)
        break
```

이 코드를 실행시키면 방금 전 코드 대비 깔끔하고 짧은 배열들을 얻을 수 있다.

```
[0, 0, 0, 30, 30, 30, 30, 30, 30]
[10, 10, 10, 40, 40, 40, 40, 40, 40]
[10, 10, 10, 40, 40, 40, 60, 60, 60]
[10, 10, 10, 40, 40, 45, 60, 60, 75]
[10, 10, 10, 40, 50, 50, 60, 80, 80]
[10, 10, 20, 40, 50, 50, 60, 80, 80]
[10, 10, 20, 40, 50, 50, 60, 80, 80]
[10, 10, 20, 50, 50, 60, 80, 90, 90]
```

첫번째로 풀었던 평범한 배낭 문제와의 차이점에 대해 알아보자.

- 배열을 물건의 무게(메모리)를 기준으로 생성하지 않고 가치(비활성화 했을 경우의 비용)를 기준으로 생성했다.
- M 바이트를 확보하기 위한 앱 비활성화의 '최소' 비용을 계산해야 했기 때문에 'kill'배열의 왼쪽부터 나가면서 M보다 같거나 큰 값을 지니고 있는 값을 탐색해나갔다.
  -M 바이트 보다 같거나 큰 값을 지니고 있는 항을 찾았으면 그 항의 index를 출력하고 break해 for문을 탈출한다.

### DP 대표 유형

- Knapsack Problem (완료)
- Longest Common Sequence
- [Longest Increasing Subsequence](https://youseop.github.io/2020-09-29-BAEKJOON-DP.1-LIS/) (완료)
- Edit distance
- Matrix Chain Multiplication

[다시 풀어보기]
<br>
<br>
