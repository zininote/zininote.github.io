---
layout: post
title: "예산"
updated: 2021-03-20
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12982?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12982?language=python3)

최대한 많은 부서에 한정된 예산을 나눠주려면, 요구예산이 적은 부서부터 순서대로 나눠주면 된다.

따라서, 부서별 요구예산을 오름차순으로 정렬한 다음, 순회를 하면서 예산을 계속 차감해 나간다. 예산이 음수가 되는 시점의 부서부터는 지원이 불가능하다. 반복문을 사용하여 예산이 0 이상일 동안의 부서 수를 리턴하는 식으로 풀었다.

## 풀이

```py
def solution(d, budget):
    #1
    a = []
    for x in sorted(d):
        if (budget := budget-x) >= 0: a += [x]
        else: break
    
    #2
    return len(a)
}
```
{:.python}

`#1` 을 보면, 부서별 요구예산 `d` 를 오름차순으로 정렬하여 순회를 하고 있다. 전체 예산 `budget` 에서 순서대로 차감을 하면서, 지원이 가능한 부서라면 `a` 리스트에 다시 삽입하고, 그렇지 못하다면 반복문을 탈출한다. Python3.8 부터 새로 소개된 할당 표현식(Assignment Expression)을 사용하였다. (:= 연산자)

`#2` 에서 지원 가능한 부서 `a` 배열의 길이를 최종리턴한다.

## 참고

뜬금없지만, 위 코드를 최대한 줄여서 나타낼 수는 없을까 고민을 하였다. 리스트 comprehension 을 사용하여 아래처럼 코딩해봤다.

```py
def solution(d, budget):
    #1
    it = sorted(d)
    return [x for x in it if (budget := budget-x) >= 0 or it.clear()].__len__()
```
{:.python}

리스트 comprehension 은 어떤 반복가능한 개체를 처음부터 끝까지 순회를 한다. 중간에 break 를 걸 수 없는 구조인데, 조건을 만족시키지 못하면 억지로 순회하는 대상을 `it.clear()` 로 없앰으로써, 강제 순회 종료가 되도록 하였다. comprehension 표현식 순회 도중 break 에 대해서는 [별도 포스팅](/post/break-list-comprehension)에도 소개했으니 참고해보기 바라다.

문제는 통과할 수 있지만 바람직한 코딩 스타일은 아닌 것 같다. itertools 모듈의 takewhile 을 쓰는 것이 제대로 된 방법이 아닐까 싶다.

## 참고2

아래는 아예 한줄로 표현해봤다.

```py
def solution(d, budget): return [x for x in setattr(solution, 'it', sorted(d)) or solution.it if (budget := budget-x) >= 0 or solution.it.clear()].__len__()
```
{:.python}

solution 함수의 속성으로 `it` 을 만들어, `d` 를 오름차순 한 리스트를 할당하였다. setattr 자체가 `it` 을 반환하지 않기에 or 연산자로 다시 묶었다.

참고로, 리스트 외 딕셔너리나 set 자료형 등은 clear 함수가 없어 comprehension 순회 중간에 break 를 걸기가 어렵다. 리스트로 바꿔서 순회해야 할 것 같다. 그리고 제너레이터라면 close 함수를 사용하면 된다.