---
layout: post
title: "순위 검색"
updated: 2021-04-18
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/72412?language=python3](https://programmers.co.kr/learn/courses/30/lessons/72412?language=python3)

처음에는 Pandas 모듈을 사용하여 쉽게 해결하려 하였으나, 효율성 테스트에서 계속 실패하였다. 관건은 쿼리문을 효율적으로 탐색하는 방법인데, 이진 탐색을 사용하여 해결하였다.

이진 탐색에 대해서는 [별도 포스팅](/post/binary-search)에 소개하였다. 여기에서 언급한 [bisect_left](https://docs.python.org/ko/3/library/bisect.html#bisect.bisect_left) 함수를 사용하였다.

## 풀이

```py
from bisect import bisect_left

def solution(info, query):
    #1
    metadata = [['cpp', 'java', 'python'], ['backend', 'frontend'], ['junior', 'senior'], ['chicken', 'pizza']]
    result = []
    
    #2
    cases = {'': []}
    for data in metadata:
        cases = {a+b: [] for a in cases.keys() for b in data}
    
    #3
    for x in (x.split() for x in info):
        cases[x[0]+x[1]+x[2]+x[3]].append(int(x[4]))
    
    #4
    cases = {k: sorted(v) for k, v in cases.items()}
    
    #5
    for x in (x.replace('and', '').split() for x in query):
        #5-1
        score = int(x[4])
        for i in range(4): x[i] = [x[i]] if x[i] != '-' else metadata[i]
        
        #5-2
        qs = {'': 0}
        for data in x[:-1]:
            qs = {a+b: 0 for a in qs.keys() for b in data}
        
        #5-3
        qs = {k: len(cases[k])-bisect_left(cases[k], score) for k, v in qs.items()}
        
        #5-4
        result.append(sum(x for x in qs.values()))
    
    #6
    return result
```
{:.python}

`#1` 에서는 변수 두개를 생성했다. `metadata` 는 실제 데이터로 사용될 값들을 저장해뒀고, `result` 는 최종결과가 저장될 리스트다.

`#2 ~ #4` 에서는 `cases` 딕셔너리를 생성하는데, `metadata` 안의 값들로 생성가능한 모든 값들을 문자열로 연결한 값을 key 로 두고, 여기에 문제에서 주어지는 `info` 를 순회하여 점수들을 리스트 형태의 value 로 둔다. `{'cppbackendjuniorchicken': [점수, 점수, ...], ...}` 형태가 된다. 그리고 점수를 이진 탐색이 가능하도록 오름차순으로 정렬해둔다.

`#5` 에서는 `query` 를 순회하면서 - 로 표시된 부분은 `metadata` 값으로 교체한다. 즉 `-frontendjuniorpizza` 가 쿼리로 주어지면 `cppfrontendjuniorpizza`, `javafrontendjuniorpizza`, `pythonfrontendjuniorpizza` 을 모두 검색한다는 의미다. 이 방식으로 `qs` 딕셔너리를 생성한 다음, 이진 탐색으로 `cases` 변수 안 value 리스트를 검색하여 일정 점수 이상 개수를 가져온다.

`#6` 에서는 생성된 `result` 리스트를 최종리턴한다.