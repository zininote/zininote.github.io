---
layout: post
title: "배달"
updated: 2021-04-17
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12978?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12978?language=python3)

다익스트라 알고리즘이 먼저 떠오르는 전형적인 문제였다. 그래프를 구현하고, 다익스트라 알고리즘으로 시작 노드에서 출발해서 다른 노드까지 도달할 수 있는 최소거리를 구하면 된다.

## 풀이

```py
from queue import PriorityQueue
import sys

def solution(N, road, K):
    #1
    g = {}
    for x, y, d in road:
        g.setdefault(x, []).append((d, y))
        g.setdefault(y, []).append((d, x))
    
    #2
    return sum(1 for x in dijkstra(1, g).values() if x <= K)

#3
def dijkstra(n, g):
    dist = {x: sys.maxsize for x in g.keys()}
    (q := PriorityQueue()).put((0, n))
    
    while not q.empty():
        d, n = q.get()
        if d < dist[n]:
            dist[n] = d
            for nd, nn in g[n]:
                q.put((d+nd, nn))
    
    return dist
```
{:.python}

`#1` 은 그래프 `g` 를 초기화하였다. `{노드: (인접노드까지의 거리, 인접노드), ..., ... }` 형태로 구현된다. 인접노드끼리 양방향으로 이동이 가능하기 때문에 x -> y 만이 아니라 y -> x 로의 접근도 그래프에 삽입했다.

`#2` 는 다익스트라 알고리즘의 결과로 반환되는 각 노드별 도달하기까지의 최소거리에서 `K` 이내인 것들의 개수를 세어 최종리턴한다.

`#3` 은 다익스트라 알고리즘을 구현한 함수다. 이 함수의 결과는 `{노드: 최소거리, ...}` 를 반환한다. 다익스트라 알고리즘에 대해선 [별도 포스팅](/post/dijkstra)에 설명했으니 참고해보기 바란다. 