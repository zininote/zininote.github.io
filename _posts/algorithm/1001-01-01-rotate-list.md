---
layout: post
title: "리스트 데이터요소 로테이션"
updated: 2021-03-27
tags: [algorithm,list]
---

## 리스트 데이터요소 로테이션

순서대로 나열되어 있는 데이터요소를 n 만큼 왼쪽 혹은 오른쪽으로 로테이션을 시킬 때가 있다. 리스트 범위 밖으로 밀려난 데이터요소는 다시 반대쪽 앞부터 채워가는 (마치 회전하는 듯한) 형태가 된다.

예를들어 아래 그림을 보면, 왼쪽은 `rotate(arr, -2)`, 오른쪽은 `rotate(arr, 2)` 와 같은 명령을 수행한 결과이다.

![그림00](/img/algorithm/algorithm-1001-01-01-00.svg)

왼쪽 그림은 왼쪽 방향으로 2 칸 이동을, 오른쪽 그림은 오른쪽 방향으로 2 칸 이동을 했다. 방향대로 이동할 수 있는 부분(하얀 부분)과, 배열 범위를 벗어나서 반대쪽 앞으로 채워지는 부분(붉은 부분)이 구분되어있다.

로테이션은 아래와 같은 특징이 있다.

```txt
n 만큼 로테이션의 결과 == 배열길이 - n 만큼 반대방향 로테이션 결과
n 이 리스트 길이 이상일 때 로테이션 결과 == n % 배열길이 로테이션 결과
```
{:.pseudo}

첫번재 특징으로 인해, 로테이션 알고리즘은 한쪽 방향만 생각해서 구현해도 된다. 그림을 봐도 왼쪽(오른쪽) 2 칸 이동의 결과는 반대방향 6 칸 이동의 결과와 같음을 알 수 있을 것이다.

두번째 특징은 마치 회전하는 듯한 로테이션 결과를 생각해보면 쉽게 이해가 된다.

## 기본 알고리즘

로테이션 결과로 리스트 범위를 벗어나는 부분을 잘라서, 반대쪽에 붙이는 식으로 리스트를 생성하면 된다.

```py
def rotate(arr, n):
    n = n%len(arr)
    return arr[-n:]+arr[:-n]

arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']

print(arr)                # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
print(rotate(arr, -2))    # ['C', 'D', 'E', 'F', 'G', 'H', 'A', 'B']
print(rotate(arr, 2))     # ['G', 'H', 'A', 'B', 'C', 'D', 'E', 'F']
```
{:.python}

먼저 알아야 할 중요한 포인트가 있다. **Python 의 % 연산자는 음수가 있으면 다른 언어들과는 다르게 동작**을 한다. [stackoverflow](https://stackoverflow.com/questions/3883004/the-modulo-operation-on-negative-numbers-in-python) 사이트 중간에 보면 % 연산자는 아래 수식과 같다고 한다.

```txt
Python: x%y = x - math.floor(x/y)*y
Others: x%y = x - math.trunc(x/y)*y
```
{:.pseudo}

보통 floor 함수는 *그 숫자를 넘지 않는 가장 큰 정수* 를, trunc 함수는 *그 숫자에서 소수점 이하를 제외한 정수* 를 리턴하는 함수다. 양수에 적용하면 두 함수는 동일한 결과를 보인다. 하지만 음수는 그 결과가 다르다.

Python 의 % 연산에 따라, 위 rotate 함수 내부에서 계산되는 `-2%8` 의 결과는 6 이 된다. 재밌는 점은, % 연산자가 `배열길이 - n` 을 구해준다는 점이다. 음수 `n` 은 본래 왼쪽 로테이션을 상정했었으므로, 자동으로 동일한 결과를 가져오는 오른쪽 로테이션으로 바뀌게 된다.

즉 오른쪽으로의 로테이션 로직만 신경쓰면 된다. 제일 마지막 `arr[-n:]+arr[:-n]` 표현식이 그것이다. (그러나 이는 왼쪽 로테이션의 경우에도 들어맞는 표현식이기는 하다.)

## Reversal 알고리즘

이 알고리즘은 [geeksforgeeks](https://www.geeksforgeeks.org/program-for-array-rotation-continued-reversal-algorithm/) 사이트 내용을 참고하였다. `rotate(arr, 2)` 를 수행한다고 하면 아래처럼 도식화할 수 있다.

![그림01](/img/algorithm/algorithm-1001-01-01-01.svg)

먼저, 오른쪽으로 로테이션이 되는 부분(하얀부분)과, Array 범위를 벗어나서 다시 왼쪽으로 붙게되는 부분(붉은부분)을 구분한다.

그리고 하얀부분끼리, 붉은부분끼리 데이터요소 순서를 뒤집는다.

그리고 다시 한번 전체 데이터요소 순서를 뒤집는다.

Python 코드로 구현하면 아래와 같다.

```py
def rotate(arr, n):
    n = n%len(arr)
    return (arr[:-n][::-1]+arr[-n:][::-1])[::-1]

arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']

print(arr)                # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
print(rotate(arr, -2))    # ['C', 'D', 'E', 'F', 'G', 'H', 'A', 'B']
print(rotate(arr, 2))     # ['G', 'H', 'A', 'B', 'C', 'D', 'E', 'F']
```
{:.python}

## deque 클래스의 rotate 함수

Python 의 collections 모듈은 deque 클래스를 제공한다. 양방향 Queue 를 나타내는 자료형으로 구체적인 사항은 [Python 공식문서](https://docs.python.org/ko/3.9/library/collections.html#collections.deque)를 참고하자. 이 클래스는 rotate 함수를 내장하고 있다.

```py
from collections import deque

arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
dq1 = deque(arr)
dq2 = deque(arr)

print(arr)        # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']

dq1.rotate(-2)
print(dq1)        # deque(['C', 'D', 'E', 'F', 'G', 'H', 'A', 'B'])

dq2.rotate(2)
print(dq2)        # deque(['G', 'H', 'A', 'B', 'C', 'D', 'E', 'F'])
```
{:.python}

위에서 직접 구현했던 rotate 함수는 원본 조작없이 복사본을 리턴하지만, deque.rotate 함수는 원본을 직접 조작하는 식으로 설계되어있다.