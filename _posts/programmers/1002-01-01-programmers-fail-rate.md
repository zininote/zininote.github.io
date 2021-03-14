---
layout: post
title: "실패율"
updated: 2021-03-14
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42889?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42889?language=python3)

문제에서 요구하는 실패율(스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수)을 구하고, 이를 정렬하여 리턴하면 된다.

## 풀이

```py
def solution(N, stages):
    #1
    p = {x: stages.count(x) for x in range(1, N+1)}
    q = {x: len(stages) - sum(p[y] for y in range(1, x)) for x in range(1, N+1)}
    
    #2
    f = {x: p[x]/q[x] if q[x] != 0 else 0 for x in range(1, N+1)}
    
    #3
    return sorted(f.keys(), key=lambda x: (-f[x], x))
```
{:.python}

`#1` 에서는 실패율 계산의 분자와 분모를 계산하여 각각 `p`, `q` 딕셔너리로 생성했다. 먼저 클리어 못한채로 그 스테이지에 머물러 있는 플레이어수를 먼저 `p` 에 할당하였다.

다음으로 스테이지에 도달한 플레이어 수는 해당 스테이지 이상에 해당하는 플레이어들 수를 구하면 되는데, 이미 구한 `p` 를 참조하여 전체 플레이어수 `len(stages)` 에서 해당 스테이지에 도달하지도 못한 플레이어수를 차감하는 식으로 구하였다. `stages` 를 순회하면서 스테이지 도달 플레이어 수를 직접 구하는 것보다는 처리 속도가 빨랐다.

`#2` 에서는 실패율을 계산하여 `f` 딕셔너리에 할당하였다.

`#3` 에서는 문제에서 요구한대로 정렬하여 최종리턴했다.