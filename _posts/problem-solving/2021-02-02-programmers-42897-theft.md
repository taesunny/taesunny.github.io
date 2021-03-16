---
title:  "[Programmers] 도둑질 (42897번) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, dp]

toc: true
toc_sticky: true
 
date: 2021-02-02
last_modified_at: 2021-02-02
---
# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42897

---

# Solution

``` java
import java.lang.Math;

class Solution {
    public int solution(int[] money) {        
        int[][] temp = new int[money.length][2];
        // index 0 - including the first element
        // index 1 - not including the first element
        
        temp[0][0] = money[0];
        temp[1][0] = money[0];
        temp[1][1] = money[1];
        
        for(int i=2; i<money.length-1; i++) {
            temp[i][0] = Math.max(money[i] + temp[i-2][0], temp[i-1][0]);
            temp[i][1] = Math.max(money[i] + temp[i-2][1], temp[i-1][1]);
        }
        
        return Math.max(Math.max(temp[money.length-2][0], temp[money.length-2][1]),
    Math.max(temp[money.length-3][1]+money[money.length-1], temp[money.length-3][0]));
    }
}
```