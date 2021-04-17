---
layout: post
title: "방문 길이"
updated: 2021-04-17
tags: [programmers,lv2]
---

##문제

[https://programmers.co.kr/learn/courses/30/lessons/49994](https://programmers.co.kr/learn/courses/30/lessons/49994)

중복을 허용하지 않는 set 자료형에, 지나온 길을 좌표화하여 삽입한 뒤, 마지막에 그 개수를 세는 방식으로 문제를 풀었다.

지나온 길 자체도 set 자료형을 사용했다. 예를들어, (0, 0) 좌표에서 (0, 1) 로 이동했다고 하면 `{(0, 0), (0, 1)}` 이라고 하였다. 이는 곧 반대방향으로의 이동인 `{(0, 1), (0, 0)}` 과도 동일하기 때문이다.

##풀이

```py
def solution(dirs):
    #1
    border, path, x, y = 5, set(), 0, 0
    
    #2
    for d in dirs:
        #2-1
        px, py = x, y
        if d in ['U']: y -= 1 if -border < y else 0
        elif d in ['R']: x += 1 if x < border else 0
        elif d in ['D']: y += 1 if y < border else 0
        elif d in ['L']: x -= 1 if -border < x else 0
        
        #2-2
        if (px, py) != (x, y):
            path |= {frozenset({(px, py), (x, y)})}
    
    #3
    return len(path)
```
{:.python}

`#1` 에서 여러 변수를 초기화 했다. `border` 는 좌표의 경계를, `path` 는 지나온 길들을, `x` 와 `y` 는 초기 좌표를 의미한다. 특히 최종결과물이 되는 `path` 는 `&#10100;&#10100;(이동전좌표), (이동후좌표)}, ... }` 형태로 set 안에 set 을 넣는 구조가 된다.

참고로, 본래 set 에는 immutable 값만 삽입 가능하므로, set 안에 set 을 넣을 순 없다. 그래서 immutable set 인 frozenset 을 사용하였다.

`#2` 에서는 본격적으로 좌표를 따라 이동시킨다. `#2-1` 부분은 입력값에 따라 좌표를 변동시키고, `#2-2` 부분은 이동의 결과 실제 좌표가 변했다면 이동을 했다는 의미이므로, 그 길을 `path` 에 frozenset 형태로 삽입한다.