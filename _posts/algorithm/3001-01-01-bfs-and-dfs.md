---
layout: post
title: "BFS (너비우선탐색) 과 DFS (깊이우선탐색) 구현 코드"
updated: 2021-03-30
tags: [algorithm,graph]
---

## 그래프 탐색

그래프의 모든 노드(정점)를 탐색하는 방식은 크게 두가지가 있다. BFS(너비우선탐색)과 DFS(깊이우선탐색)이 그것인데, 각각 무엇이고 어떤 순서로 탐색하는지, 주요 알고리즘은 무엇인지는 [나무위키](https://namu.wiki/w/%EB%84%93%EC%9D%B4%20%EC%9A%B0%EC%84%A0%20%ED%83%90%EC%83%89)나 포털 등에서 검색을 해보기 바란다.

이 포스팅에서는 BFS 와 DFS 를 코드로 어떻게 구현할 수 있는가를 중점으로 다루고자 한다. [이 pdf 문서](http://web.cs.unlv.edu/larmore/Courses/CSC477/bfsDfs.pdf) 와 [geeksforgeeks](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/) 내용을 주로 참고하였다.

여기에 있는 코드들은 모두 아래 그래프를 탐색한다. 0 번부터 8 번까지 모든 노드들을 일정한 순서대로 탐색한다.

![그래프](/img/algorithm/algorithm-3001-01-01-00.svg)
{:.center}

그리고 위 그래프를 아래 딕셔너리로 나타내기로 한다. `노드: [방문가능한 노드, 방문가능한 노드, 방문가능한 노드, ...]` 형태다.

```python
g = {
    0: [1, 3, 7],
    1: [0, 2],
    2: [1],
    3: [0, 4, 6],
    4: [3, 5],
    5: [4],
    6: [3],
    7: [0, 8],
    8: [7],
}
```

## BFS 코드

```python
def bfs(n, g, *, visited=set(), queue=[]):
    #1 시작 노드를 방문표시하고 Queue 에 삽입
    visited.add(n)
    queue.append(n)

    while queue:
        #2 Queue 에서 노드 꺼내서, 작업 진행
        n = queue.pop(0)
        print(n, end=' ')

        #3 현재 노드와 연결된 미방문 노드가 있으면 방문표시하고 Queue 에 삽입
        for x in g[n]:
            if x not in visited:
                visited.add(x)
                queue.append(x)

bfs(0, g)   # 0 1 3 7 2 4 6 8 5
```
{:.python}

bfs 함수의 인수로 시작노드 와 그래프를 지정한다. 함수 내부에서 사용할 변수들도 디폴트로 지정하는데, `visited` 는 이미 방문한 노드를 저장하고, `queue` 는 작업할 노드를 저장한다. 이름에서 유추할 수 있는데 저장된 노드들을 LIFO 방식으로 꺼내서 하나씩 처리하게 된다.

`#1` 에서 먼저 인수로 받은 시작노드를 `queue` 에 담고 `visited` 에 등록 후, queue 가 빌 때까지 (즉 모든 노드 탐색을 다 마칠 때까지) 반복을 시작한다.

`#2` 에서는 `queue` 에서 노드를 하나씩 꺼내서 하고싶은 처리를 하면 된다. 위 코드에서는 print 함수로 작업을 진행하는 노드번호를 화면에 출력만 하도록 했다.

`#3` 에서는 현재 노드에서 방문할 수 있는 노드를 순회하면서, 아직 미방문이라면 `visited` 에 노드를 방문했다고 표시를 하고 `queue` 에 삽입한다.

출력결과를 보면 이 코드가 왜 너비우선탐색인지 알 수 있다. 그리고 이는 Queue 라는 자료구조의 특색때문이기도 하다.

## DFS_iterative

DFS 방식은 크게 두가지가 있다. 하나는 반복문을 사용한 iterative 방식이고, 다른 하나는 재귀호출을 사용한 recursive 방식이다. iterative 방식을 먼저 소개한다.

```python
def dfs_iterative(n, g, *, visited=set(), stack=[]):
    #1 시작 노드를 방문표시하고 Stack 에 삽입
    visited.add(n)
    stack.append(n)

    while stack:
        #2 Stack 에서 노드 꺼내서, 작업 진행
        n = stack.pop()
        print(n, end=' ')

        #3 현재 노드와 연결된 미방문 노드가 있으면 방문표시하고 Stack 에 삽입
        for x in g[n]:
            if x not in visited:
                visited.add(x)
                stack.append(x)

dfs_iterative(0, g)   # 0 7 8 3 6 4 5 1 2
```
{:.python}

위 bfs 함수와 비교해서 바뀐 건 사실상 한가지 뿐이다. `#2` 에서 pop 메서드로 노드를 추출하는 방식이 다른데 이 때문에 탐색 순서가 달라진다. (물론 이외에도 `queue` 라는 변수명 대신 `stack` 을 사용하긴 했지만 이름을 변경한 것 뿐이다.)

다만, 익히 DFS 를 아는 분들은 탐색 순서가 좀 다르다고 느낄 수 있는데, 이 땐 `for x in g[n]:` 구문을 `for x in g[n][::-1]:` 로 바꿔서 역순으로 순회를 하도록 해보자. 그럼 출력이 0 1 2 3 4 5 6 7 8 로 나오게 된다. 어쨌든 둘 다 깊이우선탐색임에 틀림없다.

## DFS_recursive

```python
def dfs_recursive(n, g, *, visited=set()):
    def fn(n, visited):
        #2 노드 작업 진행
        print(n, end=' ')
        
        #3 현재 노드와 연결된 미방문 노드가 있으면 방문표시하고 해당 노드으로 재귀호출
        for x in g[n]:
            if x not in visited:
                visited.add(x)
                fn(x, visited)

    #1 시작 노드를 방문표시하고 재귀함수 실행
    visited.add(n)
    fn(n, visited)

dfs_recursive(0, g)     # 0 1 2 3 4 5 6 7 8
```
{:.python}

함수 안에 재귀호출을 위한 함수가 하나 더 들어있다. 모양은 좀 달라보이지만 `#1`, `#2`, `#3` 부분이 구분되어 코딩되어있는 것은 같다.

재귀호출이 함수스택을 사용하는 것이기에, Stack 자료구조를 사용하는 DFS 와 원리는 비슷하다.