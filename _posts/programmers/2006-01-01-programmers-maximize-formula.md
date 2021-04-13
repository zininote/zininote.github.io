---
layout: post
title: "수식 최대화"
updated: 2021-04-13
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/67257?language=python3](https://programmers.co.kr/learn/courses/30/lessons/67257?language=python3)

문자열로 주어진 수식을 파싱, 우선순위별로 모든 연산자 case 를 순회, 주어진 연산자 순대로 계산 수행을 구현하는 것이 핵심인 문제다.

## 풀이

```py
import re
from itertools import permutations

def solution(expression):
    #1
    ex = re.split(r'([*+-])', expression)
    cal = {
        '*': lambda x, y: x*y,
        '+': lambda x, y: x+y,
        '-': lambda x, y: x-y,
    }
    a = []
    
    #2
    for case in permutations(x for x in ['*', '+', '-'] if x in ex):
        ex_ = ex[:]
        for op in case:
            while op in ex_:
                i = ex_.index(op)
                ex_ = ex_[:i-1] + [cal[op](int(ex_[i-1]), int(ex_[i+1]))] + ex_[i+2:]
        a += [abs(ex_[0])]
        
    #3
    return max(a)
```
{:.python}

`#1` 에서는 수식문자열을 정규식으로 파싱하였다. 연산자와 숫자를 분리하여 `ex` 리스트로 만들었다. 그리고 나중에 연산기호에 맞춰 연산을 실행할 수 있도록 `cal` 딕셔너리에 연산을 담아뒀다. 마지막 `a` 는 연산자 우선순위 case 별로 연산 결과를 담을 리스트다.

`#2` 는 itertools 모듈의 permutations 함수를 사용하여 연산자 우선순위별 순열 case 를 만들어 순회한다. 각각의 case 에 따라 `ex` 의 복사본 `ex_` 를 준비하여, 연산순위 별로 연산을 시행한다.

복사본 안에 해당 연산이 없어질 때까지 반복적으로 연산을 시행하며, 연산이 모두 완료되면 `ex_` 내부에는 숫자 하나만 남게 된다. 이 숫자의 절대값을 `a` 에 삽입한다.

`#3` 에서는 `a` 중 가장 큰 값을 최종리턴한다.