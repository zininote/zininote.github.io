---
layout: post
title: "예산"
updated: 2021-03-15
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12982?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12982?language=python3)

최대한 많은 부서에 한정된 예산을 나눠주려면, 요구예산이 적은 부서부터 순서대로 나눠주면 된다.

따라서, 부서별 요구예산을 오름차순으로 정렬한 다음에, 순회를 하면서 예산을 계속 차감해 나간다. 만일 예산이 음수가 된다면, 그 부서부터는 지원이 불가능하므로, 그 이전까지의 부서 수를 리턴하는 식으로 풀었다.

## 풀이

```py
def solution(d, budget):
    #1
    count = 0
    for x in sorted(d):
        if (budget := budget-x) < 0: break
        count += 1
    return count
```
{:.python}

`#1` 를 보면, 부서별 요구예산 `d` 를 오름차순으로 정렬하여 순회하고 있다. 계속 예산을 차감해가는 데, 음수라면 반복문을 탈출하고, 그 때까지의 반복횟수(즉 지원한 부서 수) `count` 를 리턴한다.

## 참고

참고로 Python 3.8 부터 지원하는 walrus 연산자 `:=` 를 if 구문에 사용하였다. 무엇인지는 [Python 공식문서](https://docs.python.org/ko/3/whatsnew/3.8.html)를 참고하자.