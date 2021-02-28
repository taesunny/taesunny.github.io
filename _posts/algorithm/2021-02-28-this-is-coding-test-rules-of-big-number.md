---
title:  "[이것이 코딩 테스트다] 큰 수의 법칙 - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, this-is-coding-test]

toc: true
toc_sticky: true
 
date: 2021-02-28
last_modified_at: 2021-02-28
---

# 문제
- 도서 '이것이 코딩 테스트다', 한빛미디어, 나동빈 지음

---

# Solution

``` java
    static int solution(int k, int m, int[] data) {
        Arrays.sort(data);

        int first = data[data.length - 1];
        int second = data[data.length - 2];

        int secondCount = m / (k + 1);
        int firstCount = m - secondCount;

        return (first * firstCount) + (second * secondCount);
    }
```
