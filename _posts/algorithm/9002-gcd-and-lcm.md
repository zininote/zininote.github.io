---
layout: post
title: "GCD(최대공약수)와 LCM(최소공배수)를 구하는 함수"
updated: 2020-02-23
tags: [algorithm,math]
---

## gcd 와 lcm

gcd 와 lcm 은 각각 최대공약수와 최소공배수를 뜻하는 영어 약어이다. 학창시절 배웠겠지만 정확한 의미가 잘 기억나지 않는다면 나무위키의 [최대공약수](https://namu.wiki/w/%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98), [최소공배수](https://namu.wiki/w/%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98) 부분을 참고해보자.

어떤 두 자연수 x, y 가 있다고 해보자. 이 때 아래와 같은 수식이 성립한다.

> x * y = gcd(x, y) * lcm(x, y)

따라서 gcd 든, lcm 이든 어느 한쪽만 구할 수 있다면 다른 나머지도 알 수 있다. 보통 gcd 를 따로 구하고, 위 수식을 이용하여 lcm 을 구한다.

## 유클리드의 호제법

어떤 두 수 x, y (단, x > y)의 gcd 를 구하는 가장 유명한 알고리즘은 [유클리드 호제법](https://namu.wiki/w/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C%20%ED%98%B8%EC%A0%9C%EB%B2%95)이다. 링크 안에 있는 내용을 간략하게 요약하면 아래와 같다.

- gcd(x, y) == gcd(y, x&y)
- 단, x&y == 0 이면 gcd(x, y) == y

위 두 수식을 프로그램으로 옮기면 
