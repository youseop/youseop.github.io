---
layout: post
title: Programmers 섬 연결하기
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/Programmers.png
share-img: /assets/img/posting/mountain.jpg
tags: [Greedy, Programmers]
comments: true
---

## [섬 연결하기](https://programmers.co.kr/learn/courses/30/lessons/42861)

### 문제분석

섬들 사이에 다리를 놓을 때 드는 비용 정보들이 주어졌을 때, 모든 섬을 연결하는데 드는 최소 비용을 구하는 문제이다.

모든 점을 연결하는데 드는 최소 비용임으로, 탐색을 어느 점에서 시작하는지에 영향을 받지 않는다.

- 간선 정보는 dictionary를 사용해 저장했다.
- 가장 건설 비용이 적은 간선부터 건설해나가기 위해서 heapq를 사용했다.
- check리스트에 섬 방문 정보를 저장했다.
- DFS를 이용해 건설 비용이 작은 간선부터 첫 방문인 섬들에 대해서만 탐색해 나간다.

<br>

### CODE

```python
def solution(n, costs):
    #섬들 간의 다리 건설 비용 정보를 dictionary에 저장
    bridge = dict()
    for a, b, c in costs:
        if a in bridge:
            bridge[a].append((b, c))
        else:
            bridge[a] = [(b, c)]
        if b in bridge:
            bridge[b].append((a, c))
        else:
            bridge[b] = [(a, c)]

    import heapq as hq

    def short(x):
        shortest = 0
        # 섬 방문여부
        check = list(True for _ in range(n))
        point = [[0, x]]
        # 길이가 가장 짧은 간선부터 탐색해 나가기 위해서 heapq사용
        hq.heapify(point)

        while point:
            dist, a = hq.heappop(point)
            # 해당 점을 아직 탐색하지 않은 경우에만 탐색한다.
            if check[a] == True:
                check[a] = False
                shortest += dist
                if a in bridge:
                    for i, w in bridge[a]:
                        if check[i]:
                            hq.heappush(point, [w, i])
        return shortest
    # 모든 점이 최단거리로 연결되어야 하기 때문에 어떤 점에서 시작하더라도 같은 값이 나와야 한다.
    answer = short(0)

    return answer
```

<br>

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!
감사합니다.👍

[꼭 다시 풀어보기]
<br>
<br>
