---
layout: post
title: "n진수 게임"
updated: 2021-03-27
tags: [programmers.lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/17687?language=python3](https://programmers.co.kr/learn/courses/30/lessons/17687?language=python3)

진법을 변환하여, 이를 한 숫자씩 늘어트린 다음, `p` 번째 사람이 말해야할 숫자만을 골라내는 문제다. 진법 변환한 숫자를 문자열로 계속 이어붙이면서, 문자열 길이가 참석자 수 `m` 이상이 될 때마다 `m` 만큼씩 잘라내는 제너레이터를 구현하여 풀어보았다.

## 풀이

```py
import numpy as np

#1
def gen(n, m):
    i, a = 0, ''
    while 1:
        if len(a) < m:
            a += np.base_repr(i, n)
            i += 1
        else:
            yield a[:m]
            a = a[m:]

def solution(n, t, m, p):
    #2
    return ''.join(x[p-1] for x, _ in zip(gen(n, m), range(t)))
```
{:.python}

`#1` 은 제너레이터 구현 부분이다. `n` 진수 변환을 위해 numpy 모듈의 base.repr 함수를 사용하였다. 진법 변환에 대해서는 [별도 포스팅](https://zininote.github.io/post/convert-number-system-function)에 따로 정리해뒀다. 진법 변환된 숫자를 `a` 에 계속 이어붙이는데, 길이가 `m` 을 넘지 않는다면 계속 이어붙이고, 넘는다면 그만큼 잘라서 yield 한다.

`#2` 에서는 제너레이터 결과를 `t` 개 까지만 순회하고, `p-1` 위치에 해당하는 숫자만 따로 모아 최종리턴한다.