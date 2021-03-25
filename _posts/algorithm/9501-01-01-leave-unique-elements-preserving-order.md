---
layout: post
title: "리스트에서 기존 순서를 유지하면서 고유한 요소만 남기는 함수"
updated: 2021-03-25
tags: [algorithm,useful]
---

## 중복없는 고유한 요소만 남기기

리스트에서 중복되는 요소들을 제거하고 고유한 요소만 남길 때는 보통 set(집합) 자료형으로 변환하였다가, 다시 리스트로 변환해주는 방식을 많이 사용한다. 집합의 특성상 중복요소를 허용하지 않기 때문이다.

```python
def unique(arr):
    return list(set(arr))

# 사용 예시
arr = [4, 3, 3, 5, 1, 9, 1, 5, 1, 6]
print(unique(arr))    # [1, 3, 4, 5, 6, 9]
```
{:.python}

결과를 보면 확실히 중복없이 고유한 요소들만 추려지지만 기존 리스트에 입력되어있던 순서를 유지하지는 않는다. [Python 공식문서](https://docs.python.org/ko/3.8/library/stdtypes.html#set-types-set-frozenset)에서 set 자료형 부분을 봐도, **원소의 위치나 삽입 순서를 기록하지 않는다**고 되어있다.

## 기존 순서를 유지하면서 고유한 요소만 남기기

아래와 같은 코드를 사용하면, 기존 순서를 유지하면서도 고유한 요소만 추려낼 수 있다. [stackoverflow](https://stackoverflow.com/questions/1960473/get-all-unique-values-in-a-javascript-array-remove-duplicates) 사이트에 Javascript 로 올라와 있는 방법이지만, Python 코드로도 적용 가능하다.

```python
def unique(arr):
    return [x for i, x in enumerate(arr) if i == arr.index(x)]

# 사용 예시
arr = [4, 3, 3, 5, 1, 9, 1, 5, 1, 6]
print(unique(arr))    # [4, 3, 5, 1, 9, 6]
```
{:.python}

리스트의 앞부분부터 순회하면서, 해당 요소의 인덱스인 `i` 가, index 함수로 찾은 `x` 의 인덱스와 일치하는지 여부를 묻는다. 일치하지 않다면, `x` 값은 보다 앞선 순서에 이미 같은값이 위치하고 있다는 뜻이므로 중복값으로 볼 수 있어 이를 제외하고 리스트를 새로 생성한다.

앞부분부터 순회하면서 고유 요소를 찾아 다시 배열로 조립하는 방식이기 때문에, 기존 요소의 순서가 보장이 된다.