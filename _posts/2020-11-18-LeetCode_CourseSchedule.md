---
layout: post
title: LeetCode Course Schedule
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: https://i.imgur.com/7TKSpik.png
share-img: /assets/img/posting/mountain.jpg
tags: [BFS, LeetCode]
comments: true
---

## [Course Schedule](https://leetcode.com/problems/course-schedule/)

<br>

- To take course 0 you have to first take course 1, which is expressed as a pair: [0,1]
- Input: numCourses = 3, prerequisites = [[1,0],[1,2],[0,2]]

각 과목들의 선수과목이 위와 같이 주어진다. 입력을 살펴보자.
총 세 과목을 수강해야 한다. 1번 과목을 수강하기 위해서는 0번과 2번 과목을 먼저 수강해야 하고, 0번 과목을 수강하기 위해서는 2번 과목을 먼저 수강해야 한다. 2번 과목은 별도의 선수과목이 없음으로 바로 수강할 수 있다.

각각의 과목들에 대한 선수과목을 dictionary에 저장했다.

```python
for a, b in prerequisites:
            check[a] = 0
            if a in save:
                save[a].append(b)
            else:
                save[a] = [b]
```

이후 각 점들에 대해 BFS를 진행해 주었다.
이때, 주의할 점은 선수 과목이 서로 꼬여있는 경우를 생각해야 한다는 것이다.
예를 들어서 prerequisites = [[1,2],[2,1]] 이라면, 1번 과목을 수강하기 위해 2번 과목을 먼저 수강해야 하고, 2번 과목을 수강하기 위해 1번 과목을 먼저 수강해야 한다. 따라서 이 수업들은 영원히 듣지 못한다.

이렇게 while문을 다시 한번 돌아도 못듣는 과목들이 그대로 남아있는 경우를 탐지해주어야 한다.

내 경우에는 아래와 같이 리스트를 복사해 비교해 줌으로써 해결했다.

```python
while point:
            #tmp_point사용해서 선수과목이 서로 얽혀있는 경우 탐지
            tmp_point = list(point)[::]
            for _ in range(len(point)):
                a = point.popleft()
                if check[a] == 0 and a in save:
                    if all(check[i] == 1 for i in save[a]):
                        check[a] = 1
                    else:
                        point.append(a)
            #한번 다시 훑었는데도 여전히 들을 수 없는 과목이 똑같이 남아있다면,
            #이 과목들은 영원히 못듣는단 뜻이다. >> while문 탈출
            if tmp_point == list(point):
                break
```

<br>

### CODE

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        from collections import deque
        check = list(1 for _ in range(numCourses))
        #선수과목들 dictionary에 저장
        save = dict()
        for a, b in prerequisites:
            check[a] = 0
            if a in save:
                save[a].append(b)
            else:
                save[a] = [b]
        #BFS진행
        point = deque([i for i in range(numCourses)])
        while point:
            #tmp_point사용해서 선수과목이 서로 얽혀있는 경우 탐지
            tmp_point = list(point)[::]
            for _ in range(len(point)):
                a = point.popleft()
                if check[a] == 0 and a in save:
                    #선수과목들을 모두 들었으면 check에 수강했다고 표시한다.
                    #수강했음으로 deque에 다시 추가하지 않아도 된다.
                    if all(check[i] == 1 for i in save[a]):
                        check[a] = 1
                    #아직 듣지 못하는 과목이라는 뜻임으로 deque에 다시 추가한다.
                    else:
                        point.append(a)
            #한번 다시 훑었는데도 여전히 들을 수 없는 과목이 똑같이 남아있다면,
            #이 과목들은 영원히 못듣는단 뜻이다. >> while문 탈출
            if tmp_point == list(point):
                break

        if sum(check) == numCourses:
            return True
        else:
            return False
```

<br>

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!
감사합니다.👍

[꼭 다시 풀어보기]
<br>
<br>
