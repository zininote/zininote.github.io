---
layout: post
title: "삼각 달팽이"
updated: 2021-04-17
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/68645?language=python3](https://programmers.co.kr/learn/courses/30/lessons/68645?language=python3)

`n` 크기의 이차원리스트를 상정한 뒤, 제일 왼쪽 위부터, 처음에는 아래로, 다음에는 오른쪽으로, 다음에는 왼쪽 위 대각선 방향으로, 순서대로 숫자를 채워나가는 식으로 해결했다.

이차원리스트는 NumPy 모듈의 ndArray 자료구조를 사용했다.

## 풀이

```py
import numpy as np

def solution(n):
    #1
    b = np.zeros((n, n), dtype='int_')
    direction = {(1, 0): (0, 1), (0, 1): (-1, -1), (-1, -1): (1, 0)}
    
    #2
    y, x, dy, dx = 0, 0, 1, 0
    for i in range(1, n*(n+1)//2+1):
        b[y, x] = i
        ny, nx = y+dy, x+dx
        if not(0 <= ny < n and 0 <= nx < n) or b[ny, nx] != 0:
            dy, dx = direction[dy, dx]
            ny, nx = y+dy, x+dx
        y, x = ny, nx
    
    #3
    return b[b != 0].tolist()
```

`#1` 에서는 `b` 이자원리스트와, 좌표가 움직일 방향을 `direction` 딕셔너리로 생성했다. 현재의 방향을 딕셔너리에 key 로 대입하면 다음에 전환할 방향을 value 로 알려주는 식이다.

`#2` 에서는 현재 위치 `y`, `x` 와 다음으로 움직일 방향 `dy`, `dx` 초기값을 설정한 뒤, 채워질 숫자만큼 반복문을 순회하였다. 참고로, `n` 이 주어지면 `n*(n+1)//2` 만큼의 숫자를 채울 수 있다.

for 반복문 안에서는, 먼저 이자원리스트의 현재위치에 숫자를 채운 뒤, 다음 위치인 `ny`, `nx` 를 검증한다. 범위를 벗어나거나 0 이 아닌 다른 숫자가 채워져있다면 방향을 전환한다. 다음 위치로 현재 위치를 옮간다. 이를 계속 반복한다.

`#3` 마지막에서는 이자원리스트에서 0 을 모두 지운 뒤, 리스트로 전환하여 최종리턴한다.