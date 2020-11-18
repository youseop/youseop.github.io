---
layout: post
title: LeetCode - Course Schedule
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/leet.png
share-img: /assets/img/posting/mountain.jpg
tags: [BFS, LeetCode]
comments: true
---

## [Course Schedule](https://leetcode.com/problems/course-schedule/)

### CODE

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        from collections import deque
        check = list(1 for _ in range(numCourses))
        save = dict()
        for a, b in prerequisites:
            check[a] = 0
            if a in save:
                save[a].append(b)
            else:
                save[a] = [b]
        point = deque([i for i in range(numCourses)])
        while point:
            tmp_point = list(point)[::]
            for _ in range(len(point)):
                a = point.popleft()
                if check[a] == 0 and a in save:
                    if all(check[i] == 1 for i in save[a]):
                        check[a] = 1
                    else:
                        point.append(a)
            print(tmp_point, point)
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
