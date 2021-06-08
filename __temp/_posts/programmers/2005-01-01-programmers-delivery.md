---
layout: post
title: "배달"
updated: 2021-04-06
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12978?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12978?language=python3)

시작노드에서 출발해서, 최소 거리로 각 노드에 도착하는 거리를 계산하면 된다. 전형적인 다익스트라 알고리즘 문제로, 알고리즘 자체에 대해서는 [별도 포스팅](https://zininote.github.io/post/dijkstra)에 소개하였다.

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

`#1` 에서는 문제에서 주어진 그래프(노드와 경로)를 `g` 딕셔너리로 생성한다. `{노드: [(거리, 방문가능한다음노드), ...], ...}` 형태로 저장된다. 단방향이 아닌 왕복이 가능한 경로이기에 반대방향으로도 노드를 삽입해줬다.

`#2` 는 별도 함수로 구현된 다익스트라 알고리즘 결과들 중, 문제에서 요구하는 조건에 맞는 노드 개수를 세어 최종리턴한다.

`#3` 은 다익스트라 알고리즘을 구현한 외부 함수인데, 위에서 언급한 별도 포스팅이나 다른 사이트에서 소개한 다익스트라 알고리즘을 이해할 수 있다면, 쉽게 알아볼 수 있을 것이다.