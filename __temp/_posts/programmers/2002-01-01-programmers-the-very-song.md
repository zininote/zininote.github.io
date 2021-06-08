---
layout: post
title: "방금그곡"
updated: 2021-04-04
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/17683?language=python3](https://programmers.co.kr/learn/courses/30/lessons/17683?language=python3)

문자열을 파싱, 플레이시간을 계산, 플레이시간만큼 곡 재생, 주어진 곡이 재생된 곡과 일치 여부 를 순서대로 진행하는 식으로 풀어보았다.

별도의 함수를 만들었는데, 우선 # 이 붙어있는 곡조를 소문자로 바꾸는 함수 (그래야 플레이시간 등 계산이 쉽다.), 나머지는 문자열로 주어지는 시간을 정수로 바꾸는 함수다.

## 풀이

```py
from itertools import cycle
import re

def solution(m, musicinfos):
    #1
    parsed = []
    for i, (s, e, t, c) in enumerate(map(lambda x: x.split(','), musicinfos)):
        parsed += [{'t': t, 'p': (p := tti(e)-tti(s)), 'c': ''.join(x for x, _ in zip(cycle(conv(c)), range(p))), 'i': i}]
    
    #2
    a = sorted((x for x in parsed if conv(m) in x['c']), key=lambda x: (-x['p'], x['i']))
    if a: return a[0]['t']
    else: return '(None)'
    
#3
def conv(c):
    return re.sub(r'[CDFGA]#', lambda x: x.group(0)[0].lower(), c)

#4
def tti(x):
    return int(x[:2])*60 + int(x[3:])
```
{:.python}

`#1` 에서는 문자열을 파싱하여, `[{'t': 타이틀, 'p': 플레이시간, 'c': 재생된시간만큼 재생된 곡 코드, 'i': 인덱스}, ...]` 형태로 된 `parsed` 리스트에 저장을 한다. 플레이시간은 `#4` 에 정의한 tti 함수를 사용했고, 재생된 곡 코드를 구하기 위해 # 이 붙은 코드를 소문자 하나로 변환하는 `#3` 의 conv 함수를 사용하였다.

`#2` 에서 Comprehension 표현식으로 `parsed` 를 순회하면서 재생된 곡들만 고른 뒤, 이를 정렬하고, 문제에서 요구하는 대로 최종리턴했다.