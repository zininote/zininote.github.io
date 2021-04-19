---
layout: post
title: "게임 맵 최단거리"
updated: 2021-04-18
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/1844?language=python3](https://programmers.co.kr/learn/courses/30/lessons/1844?language=python3)

미로를 찾아서 목적지에 도달하되, 최단거리를 찾는 문제다. 보통 BFS 방식으로 길을 찾으면 최단거리가 되기에, BFS 탐색으로 문제를 해결했다.

BFS(너비 우선 탐색)에 대해서는 [별도 포스팅](/post/bfs-and-dfs)을 참고하기 바란다.

## 풀이

```py
import numpy as np

def solution(maps):
    #1
    maps = np.array(maps)
    (checked := set()).add((0, 0))
    (queue := []).append((0, 0, 1))
    
    #2
    while queue:
        #2-1
        y, x, i = queue.pop(0)
        maps[y, x] = i
        
        #2-2
        for ny, nx in [(y-1, x), (y, x+1), (y+1, x), (y, x-1)]:
            if -1 < ny < maps.shape[0] and -1 < nx < maps.shape[1] and (ny, nx) not in checked and maps[ny, nx] == 1:
                checked.add((ny, nx))
                queue.append((ny, nx, i+1))
    
    #3
    return -1 if maps[-1, -1] == 1 else int(maps[-1, -1])
```
{:.python}

`#1` 에서는 초기화를 한다. `maps` 는 NumPy 모듈의 ndArray 자료형으로 변환했고, BFS 탐색 순회 전에 시작지점의 좌표를 방문 check 하고, `queue` 에 좌표와 거리를 삽입한다.

`#2` 는 BFS 순회의 핵심 코드다. `#2-1` 에서는 `queue` 에서 좌표와 해당거리를 꺼내서, `maps` 의 해당 좌표에 그 거리를 표시해둔다. `#2-2` 는 다음에 탐색할 좌표를 찾아내는데, 위/오른/아래/왼 방향 중에서 범위를 벗어나지 않고, 방문 check 이력이 없으면서, 길을 나타내는 1 이 표시된 곳만 골라내어 방문 이력 check 해두고 `queue` 에 증가된 거리와 함께 삽입한다.

`#3` 에서는 목적지에 도달할 수 있었는지 여부를 목적지 좌표의 거리가 갱신되었는지 여부로 판단한다. 그 결과를 최종리턴한다.