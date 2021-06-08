---
layout: post
title: "구명보트"
updated: 2021-04-20
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42885?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42885?language=python3)

구명보트에 사람들을 태우는 나름의 규칙을 만들어 문제를 해결하였다. 아래와 같은 규칙이다.

```txt
몸무게가 가장 작은 사람 x, 몸무게가 가장 큰 사람 y 를 뽑음
x 와 y 의 몸무게 합이 구명보트 무게한계 이내라면, x 와 y 를 동시에 보트에 태우고, 아니라면 y 만 보트에 태움 (현재 몸무게가 가장 작은 x 와 같이 탈 수 없다면, 다른 어느누구와도 같이 타는 것이 불가능하기 때문)
y 만 보트에 태웠다면, 그 다음으로 몸무게가 가장 큰 사람을 다시 y 로 뽑아, 무게 이내라면 x 와 y 를, 아니라면 다시 y 만 보트에 태움
x 가 그어느 y 와도 구명보트를 같이 탈 수 없었다면 x 만 태움
```
{:.pseudo}

## 풀이

```py
from collections import deque

def solution(people, limit):
    #1
    p = deque(sorted(people))
    boat = []
    
    #2
    while p:
        x = p.popleft()
        if p:
            while p:
                y = p.pop()
                if x+y <= limit:
                    boat.append((x, y))
                    break
                else: boat.append((y,))
            else: boat.append((x,))
        else: boat.append((x,))
            
    #3
    return len(boat)
```
{:.python}

`#1` 에서는 사람들을 무게 오름차순으로 정렬한 뒤, deque 자료형으로 변환하였다. 리스트의 가장 앞은 무게가 가장 작은 x 를, 리스트의 가장 뒤는 무게가 가장 무거운 y 를 나타내고, 리스트의 앞과 뒤에서 요소의 삭제가 빈번하게 일어날 것이기에 deque 자료형을 사용한 것이다. 그리고 `boat` 리스트를 생성하는데 이는 구명보트에 올라탄 사람들을 나타낼 리스트다.

`#2` 는 이중 반복문으로 위에서 언급한 규칙을 적용하여 `boat` 에 태우는 코드다. `x` 를 뽑아서, 같이 탈 수 있는 `y` 를 물색해본뒤, 같이 타거나 혼자 타거나 한다.

`#3` 은 `boat` 의 길이를 최종리턴한다.
