---
layout: post
title: "조이스틱"
updated: 2021-04-25
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42860?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42860?language=python3)

조이스틱의 최소 가로이동수, 세로이동수를 각각 구해서 합치는 방식으로 풀어보았다.

## 풀이

```py
def solution(name):
    #1
    min_h = len(name)
    must_contain_index = [i for i, x in enumerate(name) if x != 'A']
    q = [[0]]
    while q:
        moved = q.pop(0)
        if all(x in moved for x in must_contain_index): min_h = min(len(moved)-1, min_h)
        if len(moved) < len(name):
            for nextmove in [(moved[-1]+1)%len(name), (moved[-1]-1)%len(name)]: q.append(moved+[nextmove])
    
    #2
    min_v = 0
    for x in (name[i] for i in must_contain_index):
        tmp = ord(x)-ord('A')
        min_v += min(tmp, 26-tmp)
        
    #3
    return min_h+min_v
```
{:.python}

`#1` 은 최소 가로이동수를 구하는 코드다. 가장 많은 가로이동수인 `len(name)` 을 `min_h` 에 초기화한 뒤, 보다 작은 이동수가 발견될 때마다 `min_h` 를 업데이트 하는 방식을 취했다.

이름에 A 가 아닌 곳은 무조건 거쳐야하기에 반드시 거쳐야 할 `name` 의 인덱스를 `must_contain_index` 에 담은 뒤, BFS 탐색을 응용하여 좌 또는 우로 조이스틱을 움직이고, 반드시 거쳐야할 곳을 다 거쳤는지를 판단하는 구조다.

참고로, `min_h` 를 업데이트 할 땐, `moved` 에 포함된 0 은 실제로 조이스틱을 움직이지 않는 인덱스이기에 1 을 차감하였다.

`#2` 는 최소 세로이동수를 구하는 코드다. `must_contain_index` 에 포함된 인덱스의 알파벳과 A 의 차이와, 그 차이의 26의 보수 중 작은 값을 취하였다.