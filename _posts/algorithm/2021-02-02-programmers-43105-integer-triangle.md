---
title:  "[Programmers] 정수 삼각형 (43105번) - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, programmers, dp]

toc: true
toc_sticky: true
 
date: 2021-02-02
last_modified_at: 2021-02-02
---
# 문제
- https://programmers.co.kr/learn/courses/30/lessons/43105

---

# Solution

``` java
class Solution {
    public int solution(int[][] triangle) {
        int answer = -1;
        
        for(int row=1; row<triangle.length; row++) {
            for(int col=0; col<row+1; col++) {
                if(col == 0) {
                    triangle[row][col] += triangle[row-1][col];
                } else if (col == row) {
                    triangle[row][col] += triangle[row-1][col-1];
                } else {
                    triangle[row][col] += (triangle[row-1][col] > triangle[row-1][col-1]) ? triangle[row-1][col] : triangle[row-1][col-1];
                }
            }
        }
        
        for(int i=0; i<triangle.length; i++) {
            answer = (answer < triangle[triangle.length-1][i]) ? triangle[triangle.length-1][i] : answer;
        }

        return answer;
    }
}
```