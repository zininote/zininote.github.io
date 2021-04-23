---
layout: post
title: "전화번호 목록"
updated: 2021-04-23
tags: [programmers,lv2]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42577?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42577?language=python3)

전화번호 하나하나를 Trie(트라이) 자료구조에 넣은다음, 각 번호를 탐색하면서 어떤 번호가 다른 번호 안에 포함되는지를 조사하는 방법으로 풀었다.

Trie 자료구조에 대해서는 [별도 포스팅](/post/trie-structure-and-autocompletion)을 참고해보기 바란다.

## 풀이

```py
#1
class Node:
    def __init__(self):
        self.end = False
        self.next = {}

def solution(phone_book):
    #2
    root = Node()
    for phone in phone_book:
        n = root
        for x in phone:
            if x not in n.next: n.next[x] = Node()
            elif n.next[x].end: return False
            n = n.next[x]
        if n.next: return False
        n.end = True
        
    return True
```
{:.python}

`#1` 은 Trie 자료구조가 각 번호를 저장하는데 사용할 노드다.

`#2` 에서는 `phone_book` 안에 있는 전화번호의 숫자 하나하나를 순회하면서 Trie 자료구조를 생성한다. 자료구조를 생성하면서 다른 번호 포함여부를 조사하는데, 어떤 번호가 다른 번호를 품고 있다면 즉시 False 를 최종리턴한다.

어떤 전화번호가 끝났다는 `end` 속성이 True 인 지점에 도달하거나, 아니면 전화번호가 끝났을 때 계속 이어지는 번호가 있다면, 이는 어떤 전화번호가 다른 전화번호를 포함하고 있다는 의미이다.

Trie 자료구조를 False 리턴없이 완성했다면, True 를 최종리턴한다.