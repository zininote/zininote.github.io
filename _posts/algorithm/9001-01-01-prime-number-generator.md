---
layout: post
title: "2부터 소수(Prime Number)를 순서대로 yield 하는 제너레이터"
updated: 2021-02-16
tags: [algorithm,math]
---

## 소수 구하는 알고리즘

가장 유명한 알고리즘은 [에라토스테네스의 체](https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4)이다. 링크를 타고 들어가면 각종 언어로 구현한 코드도 확인할 수 있다.

다만 아쉬운 점이 있다면, 주어진 n 이라는 숫자 이하의 소수를 구한다는 점이다. 즉, 처음부터 n 을 상정해야하는 알고리즘이다.

## 제너레이터 구현

n 이라는 숫자에 관계없이, 처음부터 소수를 구해주는 방법을 없을까 구글링 해보다가 [한 사이트](http://code.activestate.com/recipes/117119-sieve-of-eratosthenes/)에서 찾을 수 있었다. 이 코드를 살짝 바꿔 아래에 나타내봤다.

```py
def gen_prime_number():
    sieve = {}
    n = 2
    while 1:
        if n in sieve:
            for x in sieve[n]:
                sieve.setdefault(n+x, []).append(x)
            del sieve[n]
        else:
            yield n
            sieve[n*n] = [n]
        n += 1
```
{:.python}

눈여겨볼 점은 이 또한 에라토스테네스의 체 알고리즘을 사용했다는 점이다.
