---
layout: post
title: "가장 큰 정사각형 찾기"
updated: 2021-04-18
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12905?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12905?language=python3)

일부 테스트를 시간초과로 통과할 수 없었다. [geeksforgeeks](https://www.geeksforgeeks.org/maximum-size-sub-matrix-with-all-1s-in-a-binary-matrix/) 사이트에 나온 풀이법으로 풀 수 있었다.

## 풀이

```py
import numpy as np

def solution(board):
    #1
    b = np.array(board)
    
    #2
    for (y, x), e in np.ndenumerate(b[:]):
        if y == 0 or x == 0: continue
        if e == 1: b[y, x] = min(b[y-1, x], b[y-1, x-1], b[y, x-1])+1
    
    #3
    return (np.max(b)**2).__int__()
```
{:.python}

`#1` 에서 주어진 `board` 를 NumPy 모듈의 ndArray 자료형으로 변환했다. 이차원리스트의 데이터요소 순회나 큰값 찾기 등이 편해서 사용했다.

`#2` 가 가장 큰 정사각형을 구하는 핵심 알고리즘 부분이다. `board` 를 순회하다가 1 을 만난다면, 그 좌표의 바로 위,왼쪽위,왼쪽 값들 중 가장 작은 값에 1 을 더한 값으로 바꾼다. 나중에 `board` 안에서 가장 큰 값을 찾으면, 가장 큰 정사각형의 한 변의 크기와 일치한다. 반복횟수 1 번만으로 끝낼 수 있어 속도가 빠르다.

`#3` 에서는 가장 큰 정사각형의 넓이를 최종리턴한다.