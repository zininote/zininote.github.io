---
layout: post
title: "소수 만들기"
updated: 2021-03-21
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12977?language=python3](https://programmers.co.kr/learn/courses/30/lessons/12977?language=python3)

크게 두가지를 해결하면 된다. 주어진 `nums` 리스트에서 숫자를 3개 뽑아내어 만들 수 있는 모든 case 를 만드는 방법(조합)과 소수인지 아닌지를 알아내는 방법이다.

## 풀이

```py
from itertools import combinations

#1
def gen_prime_number():
    sieve = {}
    n = 2
    while True:
        if n in sieve:
            for x in sieve[n]:
                sieve.setdefault(n+x, []).append(x)
            del sieve[n]
        else:
            yield n
            sieve[n*n] = [n]
        n += 1

def solution(nums):
    #2
    it = gen_prime_number()
    primes = [x for x in it if x <= 3000 or it.close()]
    
    #3
    return [x for x in combinations(nums, 3) if sum(x) in primes].__len__()
```
{:.python}

`#1` 은 소수를 2 부터 순서대로 만들어내는 제너레이트다. [별도 포스팅](/post/permutations-and-combinations)에 소개하였으니 참고해보기 바란다.

`#2` 에서는 `#1` 의 제너레이터를 사용하여 3000 이하의 수 중 소수만 `primes` 리스트에 넣는다. 문제에서 각 수는 1000 이하이고, 3 개의 수를 뽑아낸다고 했으므로 3000 이하의 수 중에서 소수만 찾으면 된다.

또한 제너레이터는 무한히 반복하도록 되어있으나 3000 을 넘긴 시점부터는 close 함수로 제너레이터 동작을 끊도록 하였다. 이에 대해서는 [별도 포스팅](/post/break-list-comprehension)을 참고해보자.

`#3` 에서는 itertools 모듈의 combinations 함수를 사용하여 `nums` 로부터 3 개의 숫자를 뽑아내는 모든 case 를 순회하도록 했다. 조합에 대해서도 [별도 포스팅](/post/permutations-and-combinations)에 소개했으니 참고해보기 바란다.