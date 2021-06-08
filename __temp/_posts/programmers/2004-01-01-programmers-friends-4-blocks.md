---
layout: post
title: "프렌즈4블록"
updated: 2021-04-04
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/17679?language=python3](https://programmers.co.kr/learn/courses/30/lessons/17679?language=python3)

"보드에 4 블록이 있다면 위치를 표시 -> 없다면 보드 안 빈칸개수 최종리턴 -> 있다면 표시한 위치를 빈칸으로 변경 -> 블록 떨어트리기"를 순차적으로 계속 반복함으로써 해결했다.

보드를 쉽게 다루기 위해 NumPy 모듈을 사용했다.

## 풀이

```py
import numpy as np

def solution(m, n, board):
    #1
    b = np.array([[*x] for x in board])
    
    #2
    while 1:
        #2-1
        b_ = np.zeros(b.shape, dtype='int_')
        
        #2-2
        for (y, x), e in np.ndenumerate(b):
            if y == b.shape[0]-1 or x == b.shape[1]-1: continue
            box = (slice(y, y+2), slice(x, x+2))
            if e != '' and np.all(b[box] == e): b_[box] = 1
        
        #2-3
        if np.all(b_ == 0): return np.count_nonzero(b == '')
        
        #2-4
        b[b_ == 1] = ''
        
        #2-5
        for col in b.T:
            col[...] = np.concatenate((col[col == ''], col[col != '']))
```
{:.python}

`#1` 에서 보드를 NumPy 모듈의 ndArray 자료형으로 변경하여 `b` 에 할당했다.

`#2` 부터는 문제를 풀기 위해 무한반복을 사용한다.

`#2-1` 에서는 보드를 나타내는 `b` 에서 4 블록이 위치한 곳을 표시해두기 위한 더미 보드인 `b_` 를 초기화 한다.

`#2-2` 은 4 블록 여부를 조사한다. 4 개 블록의 위치를 `box` 변수에 할당한 뒤, 빈칸이 아니면서 모두 일치한다면 `b_` 의 같은 위치에 1 이라고 표시한다.

`#2-3` 은 `b_` 을 조사하는데, 모든 요소가 0 이라면 4 블록이 없었다는 의미이므로 이제까지의 빈칸을 최종리턴한다.

`#2-4` 는 `b_` 가 1 인 위치의 블록을 모두 빈칸으로 바꾼다.

`#2-5` 에서는 빈칸이 발생하면 보드 `b` 의 요소를 아래로 떨어트리는 작업을 한다. 열 방향으로 순회를 하면서, 빈칸과 빈칸이 아닌 칸의 요소를 따로 구분한 뒤 다시 결합시키는 방식을 사용했다.