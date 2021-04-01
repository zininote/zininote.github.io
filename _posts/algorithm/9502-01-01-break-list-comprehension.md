---
layout: post
title: "List Comprehension 표현식에 break 사용하기"
updated: 2021-04-01
tags: [algorithm,useful]
---

## Comprehension 표현식 개체 순회 도중 Break 사용하기

결론부터 얘기하자면 아래와 같은 구문은 에러를 발생시킨다.

```py
seq = [1, 3, 5, 7, 9, 8, 6, 4, 2]

print([x for x in seq if x < 8 or break])    # SyntaxError
```
{:.python}

break 는 Statement 이다. Expression 만 허용하는 곳에 사용할 수 없기 때문이다.

본래라면 itertools 모듈의 takewhile 함수를 사용하면 되지만, 다른 편법도 가능하다. 아래부터 이를 소개해보고자 한다.

## Comprehension 으로 리스트 순회 도중 Break

```py
seq = [1, 3, 5, 7, 9, 8, 6, 4, 2]

print([x for x in seq if x < 8])                   # [1, 3, 5, 7, 6, 4, 2]
print([x for x in seq if x < 8 or seq.clear()])    # [1, 3, 5, 7]
```
{:.python}

`seq` 를 순서대로 순회하는데, `x < 8` 이 False 가 되면 or 이후의 `seq.clear()` 구문이 실행된다. 이는 해당 리스트를 지워버리기 때문에 더 이상 순회를 할 수 없다.

마치 break 구문을 사용한 것과 비슷한데 부작용이 있다. 원본에 해당하는 `seq` 리스트는 빈 리스트가 되어버린다는 점이다.

참고로, 딕셔너리나 집합 자료형을 위와 같이 사용하면 에러가 발생한다.

## Comprehension 으로 제너레이터 순회 도중 Break

```py
def seq(i=-1):
    while 1: yield (i := i+1)
        
it = seq()        
print([x for x in it if x < 5 or it.close()])    # [0, 1, 2, 3, 4]
```
{:.python}

위 코드의 `seq` 제너레이터는 숫자를 0 부터 무한히 yield 한다. 하지만 `x < 5` 가 False 가 되는 시점에서는 `it.close()` 구문이 실행되고, 이는 제너레이터 작동을 중단시킨다. 자세한 내용은 [Python 공식문서](https://docs.python.org/ko/3/reference/expressions.html#generator.close)를 참고해보자.

## 참고

리스트 순회 Break 에는 clear 함수를, 제너레이터 순회 Break 에는 close 함수를 사용했다는 차이가 있다.

그리고 딕셔너리나 집합도 어떻게든 Comrehension 순회에서 Break 를 하고 싶다면, 제너레이터로 딕셔너리나 집합을 감싼 뒤, 위 처럼 close 함수를 사용하면 된다.