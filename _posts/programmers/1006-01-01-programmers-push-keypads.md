---
layout: post
title: "키패드 누르기"
updated: 2021-03-22
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/67256?language=python3](https://programmers.co.kr/learn/courses/30/lessons/67256?language=python3)

키패드 버튼 하나하나에 상대좌표를 부여하고, 어느손으로 눌러야할 키패드인지를 구문으로 판단하는 식으로 문제를 해결했다.

## 풀이

```py
def solution(numbers, hand):
    #1
    keypad = {
        '1': (0,0), '2': (0,1), '3': (0,2),
        '4': (1,0), '5': (1,1), '6': (1,2),
        '7': (2,0), '8': (2,1), '9': (2,2),
        '*': (3,0), '0': (3,1), '#': (3,2),
    }
    
    #2
    a, lh, rh = '', '*', '#'
    for n in numbers:
        n = str(n)
        if n in ['1', '4', '7', '*']: a, lh = a+'L', n
        elif n in ['3', '6', '9', '#']: a, rh = a+'R', n
        else:
            (nx, ny), (lx,ly), (rx,ry) = keypad[n], keypad[lh], keypad[rh]
            d = (abs(lx-nx)+abs(ly-ny)) - (abs(rx-nx)+abs(ry-ny))
            if d < 0: a, lh = a+'L', n
            elif d > 0: a, rh = a+'R', n
            else:
                if hand == 'left': a, lh = a+'L', n
                else: a, rh = a+'R', n
                    
    #3
    return a
```
{:.python}

`#1` 은 키패드별로 상대좌표를 부여한 `keypad` 딕셔너리다.

`#2` 에서는 이제까지 키패드를 누른 손, 왼손의 위치, 오른손의 위치를 각각 `a`, `lh`, `rh` 로 초기화했다. 그리고 `numbers` 를 순회하면서 누를 손을 탐색해 간다.

쉬운 것부터 if else 구문으로 탐색한다. 가운데 키패드인 경우는 상대거리 `d` 를 직접계산하고 상대거리마저 같다면 `hand` 를 참고하여 누를 손을 찾아낸다.