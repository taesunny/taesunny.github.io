---
title:  "[Programmers] 피보나치 수 (12945번) - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, programmers, fibonacci]

toc: true
toc_sticky: true
 
date: 2021-02-15
last_modified_at: 2021-02-15
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/12945

---

# 기억할 점
- stack overflow 주의 - 재귀로 풀면 O(2^n)의 시간 복잡도를 가진다.
- int 숫자의 범위도 주의 - -2,147,483,648 ~ 2,147,483,647

---

# Solution 1

``` java
class Solution {
    int data[];
    
    int getFibonacci(int target) {
        
        if(target < 2) {
            return target;
        }
        
        if(data[target] > 0) {
            return data[target];
        }
        
        data[target] = getFibonacci(target-1)%1234567 + getFibonacci(target-2)%1234567;
        
        return data[target];
    }
    
    public int solution(int n) {
        
        data = new int[n+1];
        
        return  getFibonacci(n) % 1234567;
    }
}
```

# Solution 2

``` java
class Solution {    
    public int solution(int n) {        
        int a = 0;
        int b = 1;
        n--;
        
        int answer = 0;
        
        while(n > 0) {
            answer = a+b;
            a = b;
            b = answer % 1234567;
            
            n--;
        }
        
        return answer % 1234567;
    }
}
```
