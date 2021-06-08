---
layout: post
title: "셔틀버스"
updated: 2021-04-25
tags: [programmers,lv3]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/17678?language=python3](https://programmers.co.kr/learn/courses/30/lessons/17678?language=python3)

버스와 좌석을 나타내는 이차원배열을 생성한뒤, 버스 줄에 선 사람들을 하나씩 태우는 방식을 사용하였다. 제일 마지막 버스의 제일 뒷 좌석이 비어있다면 그 버스도착시간에 도착하면 되며, 제일 뒷 좌석에 누군가가 타고 있다면 그 사람보다 1 분 먼저 오면 된다.

## 풀이

```py
import numpy as np

#1
def timetoint(t): return int(t[:2])*60 + int(t[3:])
def inttotime(i): return f'{i//60:02d}:{i%60:02d}'

def solution(n, t, m, timetable):
    #2
    tt = sorted(timetoint(x) for x in timetable)
    
    #3
    buses = np.full((n, m), '', dtype='object')
    for time, bus in zip((timetoint('09:00')+i*t for i in range(n)), buses):
        bus[...] = [(time, -1)]
    for i, x in np.ndenumerate(buses):
        if tt:
            if tt[0] <= x[0]: buses[i] = (x[0], tt.pop(0))
        else: break
    
    #4
    if (lastseat := buses[-1, -1])[1] == -1:
        return inttotime(lastseat[0])
    else:
        return inttotime(lastseat[1]-1)
```
{:.python}

`#1` 은 두 함수를 정의하였다. 시간이 문자열로 주어지므로 계산이 어렵기 때문에, 처음에 시간을 모두 정수화 하여 계산한다음, 마지막에 다시 시간문자열로 되돌리기 위함이다.

`#2` 에서는 먼저 버스대기열을 도착순서대로 정렬을 해둔다. 계산 및 비교가 쉽도록 시간문자열을 정수로 변환도 하였다.

`#3` 은 버스와 좌석을 나타내는 이차원리스트를 생성한다. NumPy 모듈의 ndArray 자료형을 사용했다. 각 버스의 좌석 수와, 총 운행대수를 계산하여 각 좌석에는 `(버스도착시간, 줄선사람도착시간)` 튜플값을 저장하고, 만일 빈 좌석이라면 `(버스도착시간, -1)` 과 같은 형태로 저장된다.

`#4` 에서는 제일 뒷 버스의 뒷 좌석을 조사하여, 자리가 비어있다면 그 버스의 도착시간을, 자리가 차있다면 그 사람 도착시간보다 1 분 일찍을 최종리턴한다.