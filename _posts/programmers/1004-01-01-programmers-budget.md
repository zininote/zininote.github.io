---
layout: post
title: "예산"
updated: 2021-03-15
tags: [programmers,lv1]
---

## 문제

[https://programmers.co.kr/learn/courses/30/lessons/12982?language=javascript](https://programmers.co.kr/learn/courses/30/lessons/12982?language=javascript)

최대한 많은 부서에 한정된 예산을 나눠주려면, 요구예산이 적은 부서부터 순서대로 나눠주면 된다.

따라서, 부서별 요구예산을 오름차순으로 정렬한 다음, 순회를 하면서 예산을 계속 차감해 나간다. 예산이 음수가 되는 시점의 부서부터는 지원이 불가능하다. 반복문을 사용하여 예산이 0 이상일 동안의 부서 수를 리턴하는 식으로 풀었다.

## 풀이

```js
function solution(d, budget) {
    // 1
    let a = [];
    for(let x of d.sort((x, y) => x-y)) {
        if((budget -= x) >= 0) a.push(x); else break;
    }
    
    // 2
    return a.length;
}
```
{:.javascript}

`// 1` 을 보면, 부서별 요구예산 `d` 를 오름차순으로 정렬하여 순회를 하고 있다. 전체 예산 `budget` 에서 순서대로 차감을 하면서, 지원이 가능한 부서라면 `a` 배열에 다시 삽입하고, 그렇지 못하다면 반복문을 탈출한다.

`// 2` 에서 지원 가능한 부서 `a` 배열의 길이를 최종리턴한다.

## 참고

비단 이 문제뿐 아니라, 다른 Javascript 문제풀이들을 보면 다들 경쟁하듯 한줄로 풀어내고는 한다. 위와 동일한 로직을 reduce 함수를 사용하여 한줄로 나타내보았다.

먼저 아래 코드는 [stackoverflow](https://stackoverflow.com/questions/36144406/how-to-early-break-reduce-method) 내용을 참고하여 응용하였음을 알리는 바이다.

```js
function solution(d, budget) {
    // 1
    return [...d.sort((x, y) => x-y)].reduce((a, x, _, s) => (budget -= x) >= 0 ? a.concat([x]) : s.splice(0) && a, []).length;
}
```
{:.javascript}

오름차순으로 정렬한 `d` 를 다시 `[...]` 로 감싸서 복사본을 만들었다. 이 복사본에 reduce 함수를 적용하였다. reduce 가 순회하는 배열 자체를 조작해야 하기 때문에 복사본을 사용하는 것이 좋다.

원코드에서 사용한 `a.push(x)` 대신 `a.concat([x])` 를 사용했다. reduce 에 적용한 함수는 누산기(코드에서는 `a` 에 해당)를 계속해서 리턴해야하는데, push 함수는 요소 삽입 후의 a 의 길이를 리턴하고, concat 함수는 다른 배열 연결 후의 배열(즉 `a`)를 리턴하기 때문이다.

예산이 음수가 되는 시점에는 reduce 가 순회하는 배열(코드에서는 `s` 에 해당) 자체를 splice 함수로 없애버린다. 그래서 강제 종료가 된다. 다만, splice 함수 자체가 잘라낸 배열을 리턴하므로 `&& a` 연산을 사용하여 `a` 가 리턴되도록 만들었다.

만 하루동안 이래저래 찾아보고 삽질하면서 만들었기에 뿌듯하기는 했지만, 가독성은 심히 떨어진다.