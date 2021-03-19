---
layout: post
title: "GCD(최대공약수)와 LCM(최소공배수)를 구하는 함수"
updated: 2020-02-23
tags: [algorithm,math]
---

## gcd 와 lcm

gcd 와 lcm 은 각각 최대공약수와 최소공배수를 뜻하는 영어 약어이다. 나무위키의 [최대공약수](https://namu.wiki/w/%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98), [최소공배수](https://namu.wiki/w/%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98) 부분을 참고해보면, 어떤 두 자연수 x, y 가 있다고 할 때 아래와 같은 수식이 성립한다.

```txt
x * y = gcd(x, y) * lcm(x, y)
```
{:.pseudo}

따라서 gcd 든, lcm 이든 어느 한쪽만 구할 수 있다면 다른 나머지도 알 수 있다. 보통 gcd 를 따로 구하고, 위 수식을 이용하여 lcm 을 구한다.

## 함수로 구현

어떤 두 수 x, y (단, x > y)의 gcd 를 구하는 가장 유명한 알고리즘은 [유클리드 호제법](https://namu.wiki/w/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C%20%ED%98%B8%EC%A0%9C%EB%B2%95)이다. 링크 안에 있는 내용을 간략하게 요약하면 아래와 같다.

```txt
gcd(x, y) == gcd(y, x%y)
단, x%y == 0 이면 gcd(x, y) == y
```
{:.pseudo}

위 두 수식을 코딩으로 옮기면 아래와 같이 나타낼 수 있다. 재귀함수를 사용했다.

```py
def gcd(x, y): return gcd(y, x%y) if x%y else y
```
{:.python}

gcd 함수가 이미 구현됐다는 전제 하에, lcm 함수도 아래처럼 구현할 수 있다.

```py
def lcm(x, y): return x*y // gcd(x, y)
```
{:.python}

참고할 점이 있다. 위에서 호제법을 요약하면서 x > y 라는 단서를 달았지만, 코드 내부에서는 이에 대한 고려가 전혀 없다. 만일 x < y 라면 `x, y = y, x%y` 의 결과는 두 수를 바꾸기에 결국에는 x > y 가 되기 때문이다.

## 여러 수의 gcd, lcm 구하기

예를들어, 자연수 a, b, c, d 의 gcd(혹은 lcm) 을 구한다고 해보자. 먼저, a 와 b 의 gcd(혹은 lcm) 를 구하고, 이 결과와 c 의 gcd(혹은 lcm) 을 구하고, 다시 이 결과와 d 의 gcd(혹은 lcm) 를 구하면 된다.
