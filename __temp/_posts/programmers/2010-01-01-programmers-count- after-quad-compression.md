---
layout: post
title: "쿼드압축 후 개수 세기"
updated: 2021-04-17
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/68936?language=python3](https://programmers.co.kr/learn/courses/30/lessons/68936?language=python3)

어느 재귀함수로 주어진 이차원리스트의 모든 요소가 1 또는 0 인지 판별하고, 아니라면 4분면으로 쪼개서 다시 재귀호출을 하는 식으로 해결했다.

이차원리스트의 편한 구현을 위해 NumPy 모듈을 사용했다.

## 풀이

```py
import numpy as np

def solution(arr):
    #1
    def fn(arr):
        if np.all(arr == 0): return np.array([1, 0])
        if np.all(arr == 1): return np.array([0, 1])
        n = arr.shape[0]//2
        return fn(arr[:n, :n]) + fn(arr[:n, n:]) + fn(arr[n:, :n]) + fn(arr[n:, n:])
    
    #2
    return fn(np.array(arr)).tolist()
```
{:.python}

`#1` 은 fn 재귀함수를 구현한 부분이다. ndArray 자료형인 `arr` 을 받아서, 모든 원소가 0 혹은 1 인지 검사하고, 아니라면 4분면을 쪼개서 다시 재귀호출한다. 그리고 각 재귀호출의 최종 결과는 덧셈으로 연결되는데, ndArray 자료형의 특성상 동일한 위치의 요소끼리 덧셈을 한다.

`#2` 에서는 `arr` 리스트를 ndArray 자료형으로 전환하여 재귀호출하고, 그 결과를 다시 리스트로 바꿔 최종리턴한다.