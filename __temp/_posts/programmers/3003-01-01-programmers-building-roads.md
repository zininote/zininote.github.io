---
layout: post
title: "경주로 건설"
updated: 2021-04-25
tags: [programmers,lv3]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/67259?language=python3](https://programmers.co.kr/learn/courses/30/lessons/67259?language=python3)

시작노드에서 다른 노드들까지 도달하는데 드는 최소의 비용을 찾아내는 다익스트라 알고리즘을 응용하여 문제를 풀었다. 좌표가 아니라 좌표와 좌표를 잇는 길을 하나의 노드로 보았다.

다익스트라 알고리즘에 대한 자세한 내용은 포털을 검색하거나 [별도 포스팅](/post/dijkstra)을 참고해보기 바란다.

## 풀이

```py
from queue import PriorityQueue

def solution(board):
    #1
    size = len(board)
    costs = {}
    q = PriorityQueue()
    q.put((0, (-1, 0, 0, 0)))
    q.put((0, (0, -1, 0, 0)))
    
    #2
    while not q.empty():
        c, (py, px, y, x) = q.get()
        if (py, px, y, x) not in costs or c < costs[(py, px, y, x)]:
            costs[(py, px, y, x)] = c
            costs[(y, x, py, px)] = c
            for ny, nx in [(y-1, x), (y, x+1), (y+1, x), (y, x-1)]:
                if 0 <= ny < size and 0 <= nx < size and (ny, nx) != (py, px) and board[ny][nx] == 0:
                    nc = 100 if py == ny or px == nx else 600
                    q.put((c+nc, (y, x, ny, nx)))
    
    #3
    goal = [(size-2, size-1, size-1, size-1), (size-1, size-2, size-1, size-1)]
    return min(costs[x] for x in goal if x in costs)
```
{:.python}

`#1` 에서는 다익스트라 알고리즘에 필요한 변수를 초기화한다. `costs` 딕셔너리는 시작노드에서 각 노드까지 건설하는데 드는 최소비용을 담을 딕셔너리다. 최소비용으로 도달할 수 있는 방법이 발견될 때마다 업데이트 된다. 그리고 노드는 `(py, px, y, x)` 형태로 나타내는데, 이는 py, px 좌표에서 y, x 좌표로 이어진 길을 뜻한다. `q` 는 우선순위큐이며, `(노드까지의건설비용, 노드)` 형태의 튜플로 노드정보를 다루게 된다.

`q` 를 보면 두개의 노드를 미리 삽입해두는데, 범위 밖에서부터 시작지점으로 들어올 수 있는 길 두개를 0 이라는 비용을 가정하여 상정한 것이다.

`#2` 는 본격적인 다익스트라 알고리즘을 실행하는 부분이다. `q` 에서 노드정보를 받아서 그 노드가 아직 `costs` 에 없거나 기존의 건설비용보다 더 낮은 건설비용이라면 `costs[노드]` 를 더 낮은비용으로 업데이트 한다. 업데이트 할 때는 사실상 동일한 길을 나타내면서 방향만 다른 노드도 동시에 업데이트 한다.

업데이트를 했다면 다음으로 갈 수 있는 노드를 탐색하고, 다음노드로의 건설비용을 계산하여, `q` 에 삽입한다.

`#3` 에서는 목적지에 도달할 수 있는 두개의 노드 중에서 비용이 더 작은 것을 최종리턴한다.