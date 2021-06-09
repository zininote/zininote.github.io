---
layout: post
title: "투 포인터/슬라이딩 윈도우 알고리즘"
updated: 2021-04-27
tags: [algorithm,list]
---

## 투 포인터(Two Pointers) 알고리즘

어떤 리스트 `seq` 가 있을 때, 이 리스트 자체를 이중루프로 구현해야 할 때가 있다.

예를들면 "2 부터 짝수들이 순서대로 있을 때, 연속된 짝수 부분합이 x 가 되는 케이스의 수" 를 구한다든지 하는 것들이다. 보통은 아래와 같이 구할 수 있을 것이다.

```py
def nestedloop(seq, n):
    #1 초기화
    result = []

    #2 반복문으로 부분합 == n 인 케이스 수집
    for s, x in enumerate(seq):
        subtotal = 0
        for e, y in enumerate(seq[s:], s+1):
            subtotal += y
            if subtotal >= n:
                if subtotal == n: result.append(seq[s:e])
                break
    
    #3 결과리턴
    return result

n = 30
seq = [x*2 for x in range(1, n//2+1)]

print(nestedloop(seq, n))
# [[2, 4, 6, 8, 10],
#  [8, 10, 12],
#  [14, 16],
#  [30]]
```
{:.python}

연속된 짝수 부분합이 30 이 되는 케이스들을 모두 구하였다. for 문 안에 for 문이 사용되는 이중루프 구문이다.

안쪽 for 구문은 비슷한 계산을 계속 반복하고 있어 비효율이다. 에를들어, 2+4+6+8+10 을 구해서 하나의 케이스를 찾았다면 다음 반복문에서는 4+6+8+10+12 를 계산하게 된다. (물론 이 케이스는 조건에 부합하지 않으므로 버려진다.) 이 두 케이스를 보면 4+6+8+10 부분은 같은 값을 가질 것이므로, 두번째 케이스를 구할 땐 첫번째 케이스에서 2 를 빼고 12 를 더하는 식으로 구해도 된다.

이 점에 착안한 것이 "투 포인터 알고리즘"이다. 부분합의 앞, 뒤에 포인터를 두고 이를 조정해가며 부분합을 만들어가는 원리다. 만일 앞, 뒤 포인터의 간격이 일정하다면 마치 창문을 밀어내는 것과 같다하여 "슬라이딩 윈도우" 알고리즘이라 한다.

## 투 포인터 예시

위 예시의 "연속된 짝수 부분합이 30 이 되는 케이스" 를 투 포인터 알고리즘으로 구한다고 해보자.

먼저 아래와 같이 초기화한다. `s` 는 부분합의 시작점을, `e` 는 부분합의 끝점을 나타내는데, Python 의 range 함수 범위 규칙과 같이 ~이상, ~미만 으로 정의했다. 그리고 이 초기상태의 부분합은 2 이다.

![그림00](/img/algorithm/algorithm-0003.svg)

이제 부분합 < 30 일 때는 `e` 를 오른쪽으로 이동(부분합 증가)하고, 부분합 >= 30 일 때는 `s` 를 오른쪽으로 이동(부분합 감소)하고자 한다. 초기상태는 부분합 < 30 이므로, `e` 를 늘려간다. `e` 가 5 인덱스를 가리킬 때, 아래처럼 부분합이 30 이 된다.

![그림00](/img/algorithm/algorithm-0004.svg)

부분합이 30 이 되는 케이스를 하나 찾았다. 이제 부분합 >= 30 이므로 `s` 를 늘린다.

![그림00](/img/algorithm/algorithm-0005.svg)

부분합 < 30 이므로 `e` 를 늘린다. 그리고 이 과정을 반복하면 아래와 같이 다시 부분합이 30 이 되는 케이스를 찾을 수 있다.

![그림00](/img/algorithm/algorithm-0006.svg)

다시 `s` 와 `e` 이동을 반복한다. 아래처럼 부분합이 30 이 되는 케이스를 더 찾을 수 있다.

![그림00](/img/algorithm/algorithm-0007.svg)
![그림00](/img/algorithm/algorithm-0008.svg)

마지막에는 아래처럼 된다.

![그림00](/img/algorithm/algorithm-0009.svg)

부분합이 30 이 되는 케이스는 모두 4 개가 된다.

## Python 코드로 구현

```py
def twopointer(seq, n):
    #1 초기화
    result = []
    s, e = 0, 1
    subtotal = sum(seq[s:e])

    #2 반복문으로 부분합 == n 인 케이스 수집
    while True:
        if subtotal < n:
            if e == len(seq): break
            subtotal += seq[e]
            e += 1
        else:
            if subtotal == n: result.append(seq[s:e])
            subtotal -= seq[s]
            s += 1

    #3 결과리턴
    return result

n = 30
seq = [x*2 for x in range(1, n//2+1)]

print(twopointer(seq, n))
# [[2, 4, 6, 8, 10],
#  [8, 10, 12],
#  [14, 16],
#  [30]]
```

nestedloop 함수와는 다르게 twopointer 함수는 반복문이 하나만 사용되었다. 시간복잡도가 O(N^2) 에서 O(N) 이 된 셈이다.

반복문 내부에는 크게 네가지 조건에 따른 각각의 처리를 구현해야 한다. `e` 이동 조건, `s` 이동 조건, 부분합 == n 일 때의 처리 조건, 반복문 종료조건이 그것이다.