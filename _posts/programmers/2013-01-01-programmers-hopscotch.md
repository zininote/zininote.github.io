---
layout: post
title: "땅따먹기"
updated: 2021-04-18
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12913?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12913?language=python3)

초기값과 일반항으로 이뤄진 점화식을 세우고, 메모이제이션을 활용한 재귀호출 (동적계획법)으로 풀었다. 메모이제이션 사용한 점화식 풀이 대해서는 [별도 포스팅](post/recurrence-recursive-and-memoization)에 소개하였으니 참고해보기 바란다.

재귀함수명을 hopscotch 라 해보자. 점화식은 아래와 같다.

```txt
초기값: hopscotch(0) = land[0]
일반항: hopscotch(n) = [
           land[n][0] + max(hopscotch(n-1)[1], hopscotch(n-1)[2], hopscotch(n-1)[3]),
           land[n][1] + max(hopscotch(n-1)[0], hopscotch(n-1)[2], hopscotch(n-1)[3]),
           land[n][2] + max(hopscotch(n-1)[0], hopscotch(n-1)[1], hopscotch(n-1)[3]),
           land[n][3] + max(hopscotch(n-1)[0], hopscotch(n-1)[1], hopscotch(n-1)[2]),
       ]
```
{:.pseudo}

과도한 재귀호출을 방지해주는 메모이제이션을 위해 functools 모듈의 [lru_cache](https://docs.python.org/ko/3/library/functools.html#functools.lru_cache) 데코레이터를 사용했다.

## 풀이

```py
from functools import lru_cache
import sys
sys.setrecursionlimit(1000000)

def solution(land):
    #1
    @lru_cache
    def hopscotch(n):
        if n == 0: return land[n]

        tmp = hopscotch(n-1)
        return [land[n][i] + max(tmp[j] for j in range(4) if i != j) for i in range(4)]
    
    #2
    return max(hopscotch(len(land)-1))
```
{:.python}

`#1` 에서는 재귀함수로 점화식을 구현하고, lru_cache 데코레이터로 메모이제이션도 구현했다.

`#2` 에서 재귀함수를 호출하고, 그 결과 중 가장 큰 값을 최종리턴한다.