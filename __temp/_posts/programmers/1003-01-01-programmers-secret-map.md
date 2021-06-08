---
layout: post
title: "비밀지도"
updated: 2021-03-15
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/17681?language=python3](https://programmers.co.kr/learn/courses/30/lessons/17681?language=python3)

두 리스트에 있는 수들을 하나씩 꺼내서, OR 연산을 하고, 이진법으로 변환한 뒤, 0 과 1 을 적절한 문자로 치환하면 된다.

## 풀이

```py
def solution(n, arr1, arr2):
    #1
    return ['{:b}'.format(x|y).zfill(n).replace('0', ' ').replace('1', '#') for x, y in zip(arr1, arr2)]
```
{:.python}

풀다보니 한줄로 해결할 수 있었다.