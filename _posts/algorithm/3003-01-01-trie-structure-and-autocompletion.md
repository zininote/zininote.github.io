---
layout: post
title: "Trie(트라이) 자료구조와 자동완성"
updated: 2021-04-23
tags: [algorithm,graph]
---

## Trie 자료구조와 자동완성

포털사이트 검색창에서 뭔가를 입력하면, 사용자가 입력하는 문자에 맞춰 예상되는 단어나 문장을 미리 보여주는데, 자동완성 기능이라고 한다.

자동완성을 구현할 때는 그래프의 일종인 Trie 자료구조를 사용한다.

만일 `['baby', 'back', 'bad', 'bank', 'box', 'boxer', 'dad', 'daddy', 'dance']` 과 같은 단어들이 Trie 자료구조에 등록되어있다고 한다면, 아래와 같은 그래프 구조를 가진다.

![그림00](/img/algorithm-3003-01-01-00.svg)

보면 알겠지만 root 로부터 단어들이 글자마다 순서대로 저장되어있다. 어떤 단어의 끝에 다다르면 end 속성을 활성화한다.

## Python 코드로 구현

Trie 도 기본적으로는 그래프이므로 Node 클래스를 정의한다. 클래스 안에는 속성이 두개 있는데, 다음 노드를 가리키는 `next` 와 이 노드에서 끝나는 단어가 있는지를 가리키는 `end` 가 있다.

다음으로 Trie 클래스를 정의한다. 단어들을 자료구조 안에 등록하는 put 메소드와, 주어진 단어조각에 맞춰 자동완성 결과를 리턴하는 getall 메소드가 있다.

```py
# 노드 클래스 정의
class Node:
    def __init__(self):
        self.end = False
        self.next = {}

# 트라이 클래스 정의
class Trie:
    def __init__(self, words):
        self.root = Node()
        for word in words: self.put(word)

    def put(self, word):
        n = self.root
        for w in word:
            if w not in n.next: n.next[w] = Node()
            n = n.next[w]
        n.end = True
    
    def getall(self, piece):
        n = self.root
        for w in piece:
            if w not in n.next: return []
            n = n.next[w]

        result = []
        q = [(n, piece)]
        while q:
            n, piece = q.pop()
            if n.end == True: result.append(piece)
            for nn in n.next:
                q.append((n.next[nn], piece+nn))

        return sorted(result)
```
{:.python}

put 함수를 보면, word 를 인수로 받아서 글자 단위로 노드를 계속 탐색해나간다. 만일 노드가 없다면 새로이 생성도 한다. 마지막 글자에 도달하면 `end` 속성을 True 로 바꾼다.

getall 함수는 주어진 단어 조각을 바탕으로, 단어 조각으로 앞으로 완성이 가능한 단어들을 DFS 탐색으로 추려내어 리턴한다. (그래프의 DFS 탐색에 대해서는 [별도 포스팅](https://zininote.github.io/post/bfs-and-dfs)을 참고하기 바란다.)

크게 세부분으로 구성되어 있다. 첫번째는 주어진 단어 조각 `piece` 의 마지막 글자까지 노드를 타고 들어간다. 만일 더이상 노드가 없다면 해당 단어 조각에 맞는 자동완성 결과는 없다는 의미이므로 빈 리스트를 리턴한다. 두번째는 `piece` 마지막 글자가 가리키는 노드부터 DFS 방식으로 다른 노드들을 탐색하고 그 결과를 `result` 리스트에 담는다. 세번째는 `result` 를 정렬한 뒤 리턴한다.

실행해 본 결과는 아래와 같다.

```py
words = ['baby', 'back', 'bad', 'bank', 'box', 'boxer', 'dad', 'daddy', 'dance']
trie = Trie(words)
print(trie.getall('b'))    # ['baby', 'back', 'bad', 'bank', 'box', 'boxer']
```
{:.python}
