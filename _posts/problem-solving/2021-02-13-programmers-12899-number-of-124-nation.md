---
title:  "[Programmers] 124 나라의 숫자 (12899번) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers]

toc: true
toc_sticky: true
 
date: 2021-02-13
last_modified_at: 2021-02-13
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/12899

---

# Solution

``` java
class Solution {
    public String solution(int n) {
        String data[] = {"4", "1", "2"};
        String answer = "";
        
        int remainder = 0;

        while(n > 0) {
            remainder = n % 3;
            n = (n-1)/3;
            
            answer = data[remainder] + answer;
        }
        
        return answer;
    }
}
```