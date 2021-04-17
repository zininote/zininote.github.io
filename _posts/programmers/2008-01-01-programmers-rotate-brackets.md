---
layout: post
title: "괄호 회전하기"
updated: 2021-04-17
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/76502?language=python3](https://programmers.co.kr/learn/courses/30/lessons/76502?language=python3)

올바른 괄호문인지를 검증하는 코드와, 주어진 문자열을 순서대로 회전시키는 코드를 구현하여 결합하면 된다.

## 풀이

```py
def solution(s):
    #1
    def isvalid(s):
        matching = {')': '(', '}': '{', ']': '['}
    
        a = []
        for x in s:
            if x in [')', '}', ']'] and a and matching[x] == a[-1]: a.pop()
            else: a.append(x)
                
        return a == []
    
    #2
    result = []
    for s_rotated in (s[-i:]+s[:-i] for i in range(len(s))):
        result.append(isvalid(s_rotated))
        
    #3
    return result.count(True)
```
{:.python}

`#1` 은 올바른 괄호인지를 검증하는 함수다. 빈 리스트 `a` 를 생성한뒤, 여기에 주어진 문자열을 한글자씩 채워간다. 단, `a` 의 마지막 인덱스에 위치한 괄호와 다음으로 들어갈 괄호가 서로 매칭이 된다면 문자열을 채우지 않고 삭제를 한다. `s` 를 끝까지 반복하고 나면 올바른 괄호였을 경우에는 모든 괄호가 사라지게 된다.

`#2` 는 문자열 rotate 결과들을 순회하면서 isvalid 함수를 호출해주는 구문이다. rotate 에 대해서는 [별도 포스팅](/post/rotate-list)에 소개하였으니 참고해보기 바란다.

`#3` 에서는 isvalid 결과가 True 인 개수를 최종리턴한다.