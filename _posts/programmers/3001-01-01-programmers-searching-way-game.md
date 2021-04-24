---
layout: post
title: "길 찾기 게임"
updated: 2021-04-25
tags: [programmers,lv3]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/42892?language=python3](https://programmers.co.kr/learn/courses/30/lessons/42892?language=python3)

주어진 정보로 이진트리를 만든 다음, 이진트리 탐색 방법 중 전위, 후위 탐색을 하면 된다.

## 풀이

```py
import sys
sys.setrecursionlimit(1000000)

#1
class Node:
    def __init__(self):
        self.data = None
        self.left = None
        self.right = None

def solution(nodeinfo):
    #2
    nodes = [(i+1, x, y) for i, (x, y) in enumerate(nodeinfo)]
    nodes.sort(key=lambda x: (-x[2], x[1]))
    
    #3
    def maketree(tree, nodes):
        if nodes:
            tree.data = nodes[0]
            if (nodes_l := [x for x in nodes[1:] if x[1] < tree.data[1]]):
                tree.left = Node()
                maketree(tree.left, nodes_l)
            if (nodes_r := [x for x in nodes[1:] if tree.data[1] < x[1]]):
                tree.right = Node()
                maketree(tree.right, nodes_r)
    tree = Node()
    maketree(tree, nodes)
    
    #4
    pre, post = [], []
    def traverse(tree):
        if tree:
            pre.append(tree.data[0])
            traverse(tree.left)
            traverse(tree.right)
            post.append(tree.data[0])
    traverse(tree)
            
    #5
    return [pre, post]
```

`#1` 에서는 이진트리의 각 노드를 나타낼 클래스를 정의했다.

`#2` 에서는 주어지는 노드 정보들을 `(노드번호, x위치, y위치)` 형태로 변환하여 `nodes` 리스트에 담은 뒤, y 위치 내림차순, x 위치 오름차순 순서로 정렬해둔다.

`#3` 은 먼저 함수를 정의하는데, 이 함수는 재귀호출로 이진트리 `tree` 를 만들어내는 역할을 한다. `nodes` 안의 0 번 인덱스를 기준으로 삼고, 좌/우로 나눠 재귀호출 하는 구조다. 사전에 정렬을 해뒀기에 0 번 인덱스는 상위 노드가 되며, 그 이하의 노드들은 모두 하위 노드가 된다.

`#4` 는 이진트리를 문제에서 요구하는대로 전위, 후위 탐색을 하고 최종리턴할 정보를 모은다. 이진트리 탐색에 대해서는 [별도 포스팅](/post/binary-tree-traversals)에 따로 소개하였다.