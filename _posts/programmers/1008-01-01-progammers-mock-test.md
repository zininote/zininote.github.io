---
layout: post
title: "모의고사"
updated: 2021-03-25
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42840?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42840?language=python3)

수포자들의 패턴과 정답을 하나하나씩 비교하여 정답수를 구한 다음, 가장 많이 정답수를 맞힌 수포자를 리턴하면 된다.

## 풀이

```py
from itertools import cycle

def solution(answers):
    #1
    supos = [
        {'n': 1, 'p': [1, 2, 3, 4, 5], 'a': 0},
        {'n': 2, 'p': [2, 1, 2, 3, 2, 4, 2, 5], 'a': 0},
        {'n': 3, 'p': [3, 3, 1, 1, 2, 2, 4, 4, 5, 5], 'a': 0},
    ]
    for x in supos:
        x['a'] = sum(1 for x, y in zip(cycle(x['p']), answers) if x == y)
        
    #2
    m = max(supos, key=lambda x: x['a'])['a']
    return sorted([x['n'] for x in supos if x['a'] == m])
```
{:.python}

`#1` 에서는 수포자들의 정보를 `[{'n': 수포자번호, 'p': 찍기패턴, 'a': 정답수}, ... ]` 형태의 `supos` 리스트를 생성한다. 정답수를 구할 땐 cycle 함수로 찍기를 무한히 돌리면서, zip 함수로 정답과 하나하나씩 비교하도록 했다.

`#2` 에서는 max 함수로 최대 정답수를 알아낸 뒤, 이 정답수와 일치하는 수포자들만을 골라서 최종리턴한다.