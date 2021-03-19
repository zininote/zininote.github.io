---
layout: post
title: "다트 게임"
updated: 2021-03-14
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/17682?language=python3](https://programmers.co.kr/learn/courses/30/lessons/17682?language=python3)

문자열로 주어지는 정보를 파싱하여, 세트별 점수를 계산한 뒤, 그 합계를 리턴하는 식으로 풀었다. 파싱에는 정규식을 사용하였다.

## 풀이

```py
import re

def solution(dartResult):
    #1
    d = re.findall(r'(\d+)([SDT])([*#]?)', dartResult)
    bonus = {'S': 1, 'D': 2, 'T': 3}
    op = {'*': 2, '#': -1, '': 1}
    
    #2
    a = [0]*3
    for i, (x, b, o) in enumerate(d):
        a[i] = int(x)**bonus[b]*op[o]
        if i > 0 and o == '*': a[i-1] *= op[o]
    
    #3
    return sum(a)
}
```
{:.python3}

`#1` 에서는 먼저 `dartResult` 를 `[[점수, 보너스, 옵션], ...]` 형태로 파싱하여 `d` 리스트로 만들었다. 그리고 보너스와 옵션 적용을 나중에 쉽게 하도록 `bo`, `op` 딕셔너리를 생성하였다.

`#2` 에서는 `d` 에 담겨있는 세트별 정보를 순회하면서, 세트별 점수를 계산, `a` 리스트에 담았다.

`#3` 에서는 `a` 를 합산하여 최종리턴하였다.

## 참고

정규식 룰은 [Python 문서](https://docs.python.org/ko/3/howto/regex.html)를 참고하였고, 테스트를 위해 [regex101](https://regex101.com/) 사이트를 사용했다.