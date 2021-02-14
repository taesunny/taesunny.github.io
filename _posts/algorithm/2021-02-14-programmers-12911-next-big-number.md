---
title:  "[Programmers] 다음 큰 숫자 (12911번) - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, programmers]

toc: true
toc_sticky: true
 
date: 2021-02-14
last_modified_at: 2021-02-14
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/12911

---

# Solution

``` java
class Solution {
    int getOneCountOfDecimal(int n) {
        int cnt = 0;
        
        while(n > 0) {
            int rest = n % 2;
            
            if(rest == 1) {
                cnt++;
            }
            
            n /= 2;
        }
        
        return cnt;
    }
    
    public int solution(int n) {
        int answer = 0;
        int targetOneCnt = getOneCountOfDecimal(n);
        
        while(n < 1_000_001) {
            n ++;
            
            if(targetOneCnt == getOneCountOfDecimal(n)) {
                return n;
            }
        }
        
        return answer;
    }
}
```