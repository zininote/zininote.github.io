---
layout: post
title: "후보키"
update: 2021-04-04
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42890?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42890?language=python3)

후보키인지 판단하기 위한 모든 case 를 순회하기 위한 조합, 최소성 여부 검사를 위한 방법, 유일성 여부 검사를 위한 방법을 찾아내는 것이 핵심인 문제였다.

데이터베이스 조작을 편하게 하기 위해 NumPy 모듈을 사용했다.

## 풀이

```py
import numpy as np
from itertools import combinations

def solution(relation):
    #1
    r = np.array(relation, dtype='object')
    keys = []
    
    #2
    for c in (c for x in range(1, r.shape[1]+1) for c in combinations(range(r.shape[1]), x)):
        if is_least(keys, c) and is_unique(r, c):
            keys += [c]
            
    #3
    return len(keys)

#4
def is_least(keys, c):
    return True if not keys else all(not(set(x) <= set(c)) for x in keys)

#5
def is_unique(r, c):
    return len(a := np.sum(r[:, c], axis=1).tolist()) == len(set(a))
```
{:.python}

`#1` 에서는 데이터베이스를 NumPy 모듈의 ndArray 자료형으로 전환하였다. 그리고 최소성과, 유일성을 만족시킨 후보키들을 담을 `keys` 리스트를 생성했다.

`#2` 에서는 조합을 생성해주는 itertools 의 combinations 함수를 사용하여, 후보키 대상이 될 수 있는 모든 case 를 순회하도록 했다. 순열과 조합에 대해서는 [별도 포스팅](/post/permutations-and-combinations)을 참고해보기 바란다.

후보키 대상을 순회하면서 최소성, 유일성 만족하는 대상을 `keys` 리스트에 추가한다. 최소성, 유일성 만족하는지 여부는 별도 함수로 나타냈다.

`#3` 에서는 `keys` 길이를 최종리턴한다.

`#4` 는 최소성을 만족시키는지 여부를 판별하는 함수다. 집합(set) 자료형의 포함관계 특성을 사용했다. `keys` 가 비어있거나 후보키 대상이 `keys` 안에 있는 모든 키들을 부분집합으로 가지고 있지 말아야 한다.

참고로 조합이 원소 개수가 작은 것부터 순회를 해야 이 함수가 제대로 작동한다. 예를들어 (3, 4) 가 `keys` 에 등록되어 있었다면 원소 개수가 더 적은 (3,) 이 유일성, 최소성을 만족한다면 (3, 4) 를 버리고 (3,) 을 취해야 하나, (3, 4), (3,) 둘 모두를 최소성을 만족시키는 것으로 판단해버린다.

`#5` 는 유일성을 만족시키는지 여부를 판별하는 함수다. 이것도 집합(set) 자료형의 중복요소 금지 특성을 사용했다. 고유한 요소만 있다면 set 자료형으로 바꿔도 요소 개수는 바뀌지 말아야 한다. 고유한 요소에 대해서는 [별도 포스팅](/post/leave-unique-elements-preserving-order) 내용도 참고해보기 바란다.