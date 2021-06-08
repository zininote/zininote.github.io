---
layout: post
title: "시저 암호"
updated: 2021-03-25
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12926?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12926?language=python3)

정규식을 사용하여 문자를 치환하는 방식으로 해결했다.

## 풀이

```py
import re

def solution(s, n):
    #1
    def fn(m):
        m = m.group(0)
        o = ord('a') if m.islower() else ord('A')
        return chr((ord(m)-o+n)%26+o)
    return re.sub(r'[a-zA-Z]', fn, s)
```
{:.python}

`#1` 을 보면, 별도의 `fn` 함수를 만들었는데 치환을 담당하는 핵심적인 부분이다. 마지막에는 re.sub 를 통해 정규식으로 알파벳만 골라내고 치환 후 최종리턴한다.