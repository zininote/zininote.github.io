---
layout: post
title: "크레인 인형뽑기 게임"
updated: 2021-03-25
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/64061?language=python3](https://programmers.co.kr/learn/courses/30/lessons/64061?language=python3)

크레인의 동작인 `moves` 를 순회하면서, 인형을 바구니에 담거나 없애가고, 없애는 동안 그 수를 그때마다 세어, 제일 마지막에 리턴하는 식으로 풀었다.

## 풀이

```py
import numpy as np

def solution(board, moves):
    #1
    a = 0
    b = np.array(board)
    c = []
    
    #2
    for m in moves:
        for x in np.nditer(b[:, m-1], op_flags=['readwrite']):
            if x != 0:
                if c and c[-1] == x:
                    del c[-1]
                    a += 2
                else: c += int(x),
                x[...] = 0
                break
    
    #3
    return a
```
{:.python}

`#1` 에서는 `a`, `b`, `c` 변수를 생성하는데, 각각 사라져 없어진 인형 수, 크레인에 담긴 인형들, 바구니를 대표한다. 특히 `b` 는 numpy 모듈의 ndarray 자료형을 사용하였다.

`#2` 에서 `moves` 를 순회하는데, 크레인에 해당하는 열을 nditer 함수로 순회토록 하였다. `x != 0` 이라면 인형이 위치하고 있다는 뜻이므로 `x[...] = 0` 으로 인형을 제거하는데, 그 전에 바구니를 조사하여, 인형을 없앨지, 바구니에 채울지를 판단하도록 하였다.

## 참고

numpy 모듈에서 배열의 다양한 순회방법에 대해서는 [Numpy 공식문서](https://numpy.org/doc/stable/reference/arrays.nditer.html)를 참고하자.